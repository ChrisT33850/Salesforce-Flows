graph TD
    START([Start]) --> GET_BU[Get BU Values<br/>Apex Action]
    GET_BU --> CASE_DET{Screen Case Details<br/>Collect case info}
    
    CASE_DET --> CHK_ASSET{Asset Required?}
    
    CHK_ASSET -->|Search Asset| ASSET_SEL{Asset Selection}
    CHK_ASSET -->|No Asset| CONTACT_CHK
    
    ASSET_SEL -->|Type-ahead Lookup| ASSIGN_ASSET1[Assign Asset<br/>from Lookup]
    ASSET_SEL -->|End User List| ASSIGN_ASSET2[Assign Asset<br/>from EU Pick]
    ASSET_SEL -->|Existing Asset| SF_ROOT[Get Root Asset]
    
    ASSIGN_ASSET1 --> SF_ROOT
    ASSIGN_ASSET2 --> SF_ROOT
    SF_ROOT --> CHK_EU{End User<br/>Required?}
    
    CHK_EU -->|From Asset| ASSIGN_EU1[Use Asset Owner<br/>as End User]
    CHK_EU -->|Search EU| EU_SEARCH{End User Selection}
    CHK_EU -->|No Change| CONTACT_CHK
    
    EU_SEARCH -->|Search Result| ASSIGN_EU2[Assign End User<br/>from Search]
    EU_SEARCH -->|Optional Search| ASSIGN_EU3[Assign End User<br/>from Optional]
    
    ASSIGN_EU1 --> GET_OPEN_CASES[Get Open Cases<br/>for Asset]
    ASSIGN_EU2 --> SCREEN_EU[Screen Asset<br/>End User]
    ASSIGN_EU3 --> GET_ENDUSER[Get End User<br/>Account]
    
    GET_OPEN_CASES --> CONTACT_CHK
    SCREEN_EU --> CONTACT_CHK
    GET_ENDUSER --> CONTACT_CHK
    
    CONTACT_CHK{Contact<br/>Source?} --> |Account Pick| ASSIGN_CONTACT1[Use Account<br/>Contact]
    CONTACT_CHK -->|Type-ahead| ASSIGN_CONTACT2[Use Looked-up<br/>Contact]
    CONTACT_CHK -->|Phone Match| ASSIGN_CONTACT3[Use Phone-matched<br/>Contact]
    CONTACT_CHK -->|New Contact| CREATE_CONTACT[Create New<br/>Contact]
    CONTACT_CHK -->|Prefilled| ASSIGN_CONTACT5[Use Prefilled<br/>Contact]
    CONTACT_CHK -->|No Contact| NO_CONTACT[No Contact Selected]
    
    ASSIGN_CONTACT1 --> GET_CONTACT[Get Chosen<br/>Contact Details]
    ASSIGN_CONTACT2 --> GET_CONTACT
    ASSIGN_CONTACT3 --> GET_CONTACT
    CREATE_CONTACT --> GET_CONTACT
    ASSIGN_CONTACT5 --> GET_CONTACT
    NO_CONTACT --> GET_CONTACT
    
    GET_CONTACT --> POPULATE[Populate Case<br/>Fields]
    
    POPULATE --> CHK_VOICE{Voice Call<br/>Processing?}
    
    CHK_VOICE -->|Yes| PROCESS_VOICE[Process Voice<br/>Call Record]
    CHK_VOICE -->|No| SUMMARY
    
    PROCESS_VOICE --> SUMMARY{Review Case<br/>Summary}
    
    SUMMARY -->|Confirm| CREATE_CASE[Create Case<br/>Record]
    SUMMARY -->|Edit| CASE_DET
    
    CREATE_CASE --> LOG_OUTPUT[Log Case ID<br/>Output]
    
    LOG_OUTPUT --> SUCCESS([Case Created<br/>Success])
    
    CASE_DET -->|Error| FAULT_HANDLER[Capture Fault<br/>Message]
    FAULT_HANDLER --> ERROR_SCREEN[Display Error<br/>Screen]
    ERROR_SCREEN --> END_ERROR([End with Error])
    
    style START fill:#90EE90
    style SUCCESS fill:#90EE90
    style END_ERROR fill:#FFB6C1
    style ERROR_SCREEN fill:#FFE4E1
    style GET_BU fill:#87CEEB
    style CREATE_CASE fill:#FFD700
    style SUMMARY fill:#FFFFE0
    style CONTACT_CHK fill:#FFFFE0
    style CHK_ASSET fill:#FFFFE0
    style CHK_EU fill:#FFFFE0
    style CHK_VOICE fill:#FFFFE0
    style ASSET_SEL fill:#FFFFE0
    style EU_SEARCH fill:#FFFFE0
