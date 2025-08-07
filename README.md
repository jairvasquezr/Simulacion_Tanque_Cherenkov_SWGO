# Simulacion_Tanque_Cherenkov_SWGO
El proyecto SWGO se encuentra en estapa de evaluación de configuraciones de detectores Cherenkov para optimizar su desempeño en la detección de partículas provientes de lluvias atmosféricas. En el presente proyecto se implementó una simulación inicial de un detector Cherenkov adaptable a los requerimientos de SWGO haciendo uso del Framework MEIGA.

<img width="1022" height="745" alt="swgo" src="https://github.com/user-attachments/assets/a21eef31-17bd-49e0-bc62-7ad68c002f69" />

## Descripción del Proyecto
Este repositorio incluye la implementación completa de la simulación de un detector Cherenkov de agua, desarrollado bajo el framework Meiga y Geant4 para el experimento SWGO (Southern Wide-field Gamma-ray Observatory). El objetivo es modelar la cadena de simulación desde la generación de lluvias de partículas hasta la detección de fotones Cherenkov en un tanque de agua cilíndrico.

## Estructura del Contenido

- **5. Simulación de un detector Cherenkov de agua**
  - Descripción general de Geant4 y Meiga como herramientas de simulación fileciteturn0file0
- **5.1 Construcción del volumen mundo**
  - Definición de un cubo de 5 m de lado que contiene toda la geometría del detector. Variables `fWorldSizeX`, `fWorldSizeY`, `fWorldSizeZ`, y uso de `G4Box`, `G4LogicalVolume` y `G4PVPlacement` para crear el espacio de simulación.
    
    <img width="383" height="363" alt="mundo" src="https://github.com/user-attachments/assets/e0114669-8418-41e1-8242-be59f1d8df82" />

- **5.2 Construcción de las propiedades del PMT**
  - Modelado del Hamamatsu R5912 como un elipsoide (semiejes X=10.1 cm, Y=10.1 cm, Z=6.5 cm).
  - Definición del material Pyrex (SiO₂ 80%, B₂O₃ 13%, Na₂O 7%) y sus propiedades ópticas (`RINDEX`, `ABSLENGTH`) fileciteturn0file0
  - Implementación de la eficiencia cuántica del fotocátodo en función de la longitud de onda (250–700 nm) fileciteturn0file0
- **5.3 Construcción de las propiedades del Tyvek**
  - Material HDPE para el revestimiento interno del tanque.
  - Tabla de absorción y reflectividad difusa para maximizar la captura de luz Cherenkov (longitud de absorción = 10 m). Arreglos `water1PhotonEnergy` y `linerAbsLen`.
  - 
<img width="408" height="368" alt="Cilindro" src="https://github.com/user-attachments/assets/7784360a-6e89-4686-a30a-b95fd197abfd" />
- **5.4 Construcción del detector**
- 
  - Dimensiones del tanque: radio=105 cm, altura=90 cm, grosor pared=12 mm.
  - Creación de sólidos (`G4Tubs`) para el volumen de agua, tapa superior, tapa inferior y pared interna.
  - Asociación de materiales y posicionamiento dentro del mundo: `G4LogicalVolume` y `G4PVPlacement` para cada componente (tanque, tapas, PMT). Registro de detectores sensibles (`G4MPMTAction`, `G4MDetectorAction`)
    
<img width="779" height="267" alt="cili_pmt" src="https://github.com/user-attachments/assets/c5ac0d44-583d-4868-9d68-4aa656bb9819" />

## Resultados de la Simulación
- Inyección de un muón que atraviesa la tapa superior del tanque.
- Generación de fotones Cherenkov (visualizados en verde) reflejados por Tyvek y captados por el PMT en la parte superior central. La simulación muestra la trayectoria del muón y la posterior detección de fotoelectrones fileciteturn0file0

<img width="637" height="303" alt="wcd_muon" src="https://github.com/user-attachments/assets/ea4885b2-808a-4d43-b351-4a6cb226cd84" />

## Conclusiones
- Implementación exitosa de toda la cadena de simulación: generación de lluvias de partículas, definición geométrica y física del detector y simulación de la interacción de partículas con producción de radiación Cherenkov.
- Ventaja de versatilidad para explorar configuraciones de detectores y optimización de eficiencia y sensibilidad.
- Proyecto futuro: adaptar la simulación a detectores propuestos para SWGO y análisis detallado de datos simulados fileciteturn0file0

## Apéndice: Instalación y Configuración

### Geant4 (v11.2.2)
```bash
sudo apt update && sudo apt upgrade
wget https://gitlab.cern.ch/geant4/geant4/-/archive/v11.2.2/geant4-v11.2.2.tar.bz2
tar -xjf geant4-v11.2.2.tar.bz2
mkdir geant4-build && cd geant4-build
ccmake ../geant4-v11.2.2
# Activar opciones necesarias y establecer CMAKE_INSTALL_PREFIX=/usr/local
make -j8
sudo make install
```

### MEIGA
```bash
# Requerimientos: Geant4 v11.2.2, CMake ≥3.16, X11 OpenGL
sudo apt-get install libboost-all-dev nlohmann-json3-dev view3dscene
mkdir build install && cd build
cmake -DCMAKE_INSTALL_PREFIX=../install ../src
make -j$(nproc)
make install
```
