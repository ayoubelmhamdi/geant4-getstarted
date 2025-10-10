# HELLO WORD USING GEANT4

Popular GEANT4 prefix

| Prefix        | Meaning / Usage                        | Example                 |
|---------------|----------------------------------------|-------------------------|
| **G4**        | General                                | `G4Box`                 |
| **G4V**       | return Virtual (must be implemented)   | `G4VPhysicalVolume`     |
| **G4VPV**     | return virtual Physic volume           | `G4VPVParameterisation` |
| **G4UI**      | User interface                         | `G4UIcommand`           |
| **G4Nist**    | NIST material database                 | `G4NistManager`         |
| **G4PV**      | Physical volume (placement in space)   | `G4PVPlacement`         |
| **G4Em**      | Electromagnetic physics                | `G4EmProcessOptions`    |


this popular words may help to start codding with Geant4.

`G4Box` `G4LogicalVolume` `G4PVPlacement` `G4RunManager` `G4ThreeVector` `G4VPhysicalVolume` `Construct` `G4VUserDetectorConstruction` `Instance` `runManager` `worldBox` `worldLogic`

And may we create others likes: `MinimalDetector`, `air`, `worldBox`, `worldLogic`

an examples of instances classes.

```cpp
#if 0
    new G4Box("World", 1*m, 1*m, 1*m);
    new G4LogicalVolume(box, air, "world");
    new G4PVPlacement(
            0,                //G4RotationMatrix* pRot,
            G4ThreeVector(),  //G4ThreeVector& tlate,
            curentLogical,    //G4LogicalVolume* pCurrentLogical,
            "World",          //G4String& pName,
            NULL,             //G4LogicalVolume* pMotherLogical,
            false,            //G4bool pMany,
            false,            //G4int  pCopyNo,
            false             //G4bool pSurfChk = false
        );
#endif
```

this previous instances cames from Geant4 header files, and we listed some of them here to help creating a minimal class:

```cpp
G4NistManager::Instance()->FindOrBuildMaterial("G4_AIR");

new G4Box(
        const G4String& pName,
        G4double pX,
        G4double pY,
        G4double pZ
    );

new G4LogicalVolume(
        G4VSolid* pSolid,
        G4Material* pMaterial,
        const G4String& name
    );

new G4PVPlacement(
        G4RotationMatrix* pRot,            //G4RotationMatrix* pRot,
        G4ThreeVector& tlate,              //G4ThreeVector& tlate,
        G4LogicalVolume* pCurrentLogical,  //G4LogicalVolume* pCurrentLogical,
        G4String& pName,                   //G4String& pName,
        G4LogicalVolume* pMotherLogical,   //G4LogicalVolume* pMotherLogical,
        G4bool pMany,                      //G4bool pMany,
        G4int  pCopyNo,                    //G4int  pCopyNo,
        G4bool pSurfChk = false            //G4bool pSurfChk = false;
    );
```

we can have a `Target` in this `World` by:
```cpp
    G4Material* aluminum = nist->FindOrBuildMaterial("G4_Al");
    G4Box* targetBox = new G4Box("Target", 10*cm, 10*cm, 1*cm);
    G4LogicalVolume* targetLogic = new G4LogicalVolume(targetBox, aluminum, "Target");
```

we can always check the [ NIST Reference ](./references/nist.md)


### Add a detector - silicon behind target:

```cpp
    G4Box* detectorBox = new G4Box("Detector", 15*cm, 15*cm, 0.5*cm);
    G4LogicalVolume* detectorLogic = new G4LogicalVolume(detectorBox, silicon, "Detector");
    new G4PVPlacement(
            0,
            G4ThreeVector(0,0,20*cm),  // 20cm behind target
            detectorLogic,
            "Detector", 
            worldLogic,
            false,
            0
        );
```
