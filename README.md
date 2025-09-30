```mermaid
sequenceDiagram
    participant User
    participant System as Python Script
    participant FS as .s1p Files
    participant Preproc as S11 Preprocessing
    participant Peaks as Peak Extraction
    participant Dataset as Dataset (Pandas)
    participant Model as RandomForest
    participant Sweep as New Sweep
    participant Visualization as Matplotlib

    User->>System: Execute script

    rect rgb(200, 240, 255)
    note over System: Training
    System->>FS: select_files_by_ranges()
    loop Files without crack
        System->>Preproc: read_s1p_custom()
        Preproc->>Peaks: extract_max_with_freq()
        System->>Dataset: Save features (label=0)
    end
    loop Files with crack
        System->>Preproc: read_s1p_custom()
        Preproc->>Peaks: extract_max_with_freq()
        System->>Dataset: Save features (label=1)
    end
    System->>Model: fit(X_train, y_train)
    end

    rect rgb(220, 255, 220)
    note over System: Test / Sweep Prediction
    System->>Sweep: select_files_by_ranges()
    loop New sweep files
        System->>Preproc: read_s1p_custom()
        Preproc->>Peaks: extract_max_with_freq()
        System->>Model: Compute probability
        System->>Model: Obtain prediction
    end
    System->>Visualization: Plot results (probability and prediction)
    System->>Visualization: Plot detected maxima in sweep (ΔS11)
    System->>Visualization: Plot peak frequencies in sweep
    end
