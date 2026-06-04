# Ciclo de Vida de Predicciones

```mermaid
flowchart TD
  A["Usuario entra a Predicciones"] --> B{"Tab"}
  B --> D["Predicciones del dia<br/>tab principal"]
  B --> C["Predicciones generales<br/>tab secundario"]

  C --> E{"Generales abiertas"}
  E -->|Si| F["Usuario selecciona campeon,<br/>finalistas, clasificados,<br/>mas goles y mejor defensa"]
  F --> G["Guardar predicciones generales"]
  G --> H["Predicciones generales guardadas"]
  E -->|No| I["Mostrar generales bloqueadas"]
  H --> J{"Admin cierra o fecha limite pasa"}
  J -->|Si| I
  J -->|No| F

  D --> K["Mostrar partidos del dia"]
  K --> L{"Faltan mas de 30 min<br/>para este partido"}
  L -->|Si| M["Controles editables por partido"]
  M --> N["Guardar prediccion del dia"]
  N --> O["Prediccion del dia guardada"]
  L -->|No| P["Partido bloqueado"]
  P --> Q["Mostrar controles deshabilitados"]
```
