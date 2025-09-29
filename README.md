sequenceDiagram
    participant Usuario
    participant Sistema as Script Python
    participant FS as Archivos .s1p
    participant Preproc as Preprocesado S11
    participant Picos as Extracción de Picos
    participant Dataset as Dataset (Pandas)
    participant Modelo as RandomForest
    participant Barrido as Barrido nuevo
    participant Visualizacion as Matplotlib

    Usuario->>Sistema: Ejecutar script

    rect rgb(200, 240, 255)
    note over Sistema: Entrenamiento
    Sistema->>FS: seleccionar_archivos_por_rangos()
    loop Archivos sin grieta
        Sistema->>Preproc: read_s1p_custom()
        Preproc->>Picos: extract_max_with_freq()
        Sistema->>Dataset: Guardar características (label=0)
    end
    loop Archivos con grieta
        Sistema->>Preproc: read_s1p_custom()
        Preproc->>Picos: extract_max_with_freq()
        Sistema->>Dataset: Guardar características (label=1)
    end
    Sistema->>Modelo: fit(X_train, y_train)
    end

    rect rgb(220, 255, 220)
    note over Sistema: Predicción en test/barrido
    Sistema->>Barrido: seleccionar_archivos_por_rangos()
    loop Archivos barrido nuevo
        Sistema->>Preproc: read_s1p_custom()
        Preproc->>Picos: extract_max_with_freq()
        Sistema->>Modelo: predict(características)
        Sistema->>Modelo: predict_proba(características)
    end
    Sistema->>Visualizacion: Graficar resultados suavizados (uniform_filter1d)
    Sistema->>Visualizacion: Graficar máximos detectados en el barrido (ΔS11)
    Sistema->>Visualizacion: Graficar frecuencias de los picos en el barrido
    end
