## G4NistManager Class Reference

- **Static Public Functions**
  - Constructor & Destructor
  - Element Functions (G4Element)
    - GetElement (size_t index)
    - FindOrBuildElement (G4int Z, G4bool isotopes=true)
    - FindOrBuildElement (const G4String &symb, G4bool isotopes=true)
    - GetNumberOfElements ()
    - GetZ (const G4String &symb)
    - GetNistElementNames () const
  
  - Isotope/Mass/Energy Functions
    - GetAtomicMassAmu (const G4String &symb) const
    - GetAtomicMassAmu (G4int Z) const
    - GetIsotopeMass (G4int Z, G4int N) const
    - GetAtomicMass (G4int Z, G4int N) const
    - GetTotalElectronBindingEnergy (G4int Z) const
    - GetMeanIonisationEnergy (G4int Z) const
    - GetIsotopeAbundance (G4int Z, G4int N) const
    - GetNistFirstIsotopeN (G4int Z) const
    - GetNumberOfNistIsotopes (G4int Z) const
  
  - Material Functions (G4Material)
    - GetMaterial (size_t index)
    - GetNumberOfMaterials ()
    - GetNistMaterialNames () const
    - ListMaterials (const G4String &)
    - Find or Build Material
      - FindOrBuildMaterial (const G4String &name, ...)
      - BuildMaterialWithNewDensity (name, basename, density, temp, pres)
    - Construct New Material
      - ConstructNewMaterial (name, elements, nbAtoms, dens, ...)
      - ConstructNewMaterial (name, elements, weight, dens, ...)
      - ConstructNewGasMaterial (name, nameNist, temp, pres, ...)
      - ConstructNewIdealGasMaterial (name, elements, nbAtoms, ...)
      - Deletes materials in G4MaterialTable
      - Deletes elements in G4ElementTable
      - Deletes isotopes in G4IsotopeTable
  
  - Utility/Printing/Control
    - GetVerbose ()
    - SetVerbose (G4int)
    - PrintElement (G4int Z)
    - PrintElement (const G4String &)
    - PrintG4Element (const G4String &)
    - PrintG4Material (const G4String &)
  
  - Mathematical Utilities (Z/A)
    - GetZ13 (G4double Z)
    - GetZ13 (G4int Z)
    - GetA27 (G4int Z)
    - GetLOGA (G4double A)
    - GetLOGA (G4int Z)
    - GetLOGZ (G4int Z)
