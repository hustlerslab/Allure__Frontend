```mermaid
stateDiagram-v2

    [*] --> Discovery

    Discovery --> TemplateSelection
    TemplateSelection --> Customization
    Customization --> AIAnalysis
    AIAnalysis --> DesignerMatching
    DesignerMatching --> Quotation
    Quotation --> Approval
    Approval --> Payment
    Payment --> Execution
    Execution --> Handover

    Payment --> Payment : Retry
    Payment --> JourneyPaused : Failure

    JourneyPaused --> Payment : Resume

    Handover --> Completed

    Completed --> [*]
```
