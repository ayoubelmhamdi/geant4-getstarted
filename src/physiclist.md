# physicsList

`SetUserInitialization(physicsList)` connect the run manager to a concrete physics configuration by registering particles and processes.

### Starting point - reference physics list

```cpp
#include "G4PhysListFactory.hh"
#include "G4VModularPhysicsList.hh"

G4PhysListFactory factory;
G4VModularPhysicsList* physicsList = factory.GetReferencePhysList("QGSP_BIC_HP");
runManager->SetUserInitialization(physicsList);
```

The factory knows about pre-built reference physics lists like `QGSP_BIC_HP`, `FTFP_BERT`, `QGSP_BERT`. Each name encodes hadronic models and EM options.

### What happens inside

When `SetUserInitialization` is called, Geant4 invokes two methods on the physics list:

```cpp
// called by G4RunManager
physicsList->ConstructParticle(); // defines particles: e-, e+, gamma, proton, neutron...
physicsList->ConstructProcess();  // assigns processes: ionization, bremsstrahlung, decay...
```

Reference lists handle both automatically. Transportation gets added to all particles.

### Changing EM option

```cpp
G4PhysListFactory factory;
G4VModularPhysicsList* physicsList = factory.GetReferencePhysList("QGSP_BIC_HP_EMZ");
runManager->SetUserInitialization(physicsList);
```

Appending `_EMZ` or `_EMY` replaces the default EM module with alternatives.

### Manual physics list - starting minimal

Instead of reference lists, inherit from `G4VUserPhysicsList`:

```cpp
class MinimalPhysicsList : public G4VUserPhysicsList {
public:
    MinimalPhysicsList() {}
    ~MinimalPhysicsList() {}
    
    void ConstructParticle() {
        G4Gamma::Definition();
        G4Electron::Definition();
    }
    
    void ConstructProcess() {
        AddTransportation();  // mandatory for all particles
    }
};
```

This builds only gamma and electron with no interactions except transportation.

### Add EM processes manually

```cpp
void MinimalPhysicsList::ConstructProcess() {
    AddTransportation();
    
    G4PhysicsListHelper* ph = G4PhysicsListHelper::GetPhysicsListHelper();
    
    // ionization for electron
    ph->RegisterProcess(new G4eIonisation(), G4Electron::Definition());
    
    // bremsstrahlung for electron  
    ph->RegisterProcess(new G4eBremsstrahlung(), G4Electron::Definition());
    
    // Compton scattering for gamma
    ph->RegisterProcess(new G4ComptonScattering(), G4Gamma::Definition());
}
```

`G4PhysicsListHelper` attaches processes to particle definitions.[2]

### Using modular physics list

`G4VModularPhysicsList` allows registering pre-made physics modules instead of individual processes:

```cpp
#include "G4VModularPhysicsList.hh"
#include "G4EmStandardPhysics.hh"
#include "G4DecayPhysics.hh"

class ModularPhysicsList : public G4VModularPhysicsList {
public:
    ModularPhysicsList() {
        RegisterPhysics(new G4EmStandardPhysics());
        RegisterPhysics(new G4DecayPhysics());
    }
};
```

Each module handles a category: `G4EmStandardPhysics` adds all EM processes for leptons and photons, `G4DecayPhysics` adds decay channels. Transportation is added automatically.

### Adding hadronic physics module

```cpp
#include "G4HadronPhysicsQGSP_BIC.hh"

ModularPhysicsList() {
    RegisterPhysics(new G4EmStandardPhysics());
    RegisterPhysics(new G4DecayPhysics());
    RegisterPhysics(new G4HadronPhysicsQGSP_BIC());
}
```

`G4HadronPhysicsQGSP_BIC` adds hadronic interactions for protons, neutrons, pions using specific models (Quark Gluon String Precompound, Binary Cascade).

### Setting production cuts

```cpp
void ModularPhysicsList::SetCuts() {
    SetCutsWithDefault();  // 1mm default
    // or
    SetCutValue(0.1*mm, "gamma");
    SetCutValue(0.1*mm, "e-");
    SetCutValue(0.1*mm, "e+");
}
```

Cuts define the production threshold for secondary particles. Lower cuts increase precision but slow simulation.

