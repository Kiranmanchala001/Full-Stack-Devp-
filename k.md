graph TD
    Start([Each morning on the next day of month updated by The Admin assistants]) --> GetInvoices[Get Invoices and Contact status from BRAID/CRM for New customers]
    GetInvoices --> CheckStatus{Check Contact Customer Status within Monday Tool}

    %% Far Left Path: Balance = Nil
    CheckStatus -->|Balance = Nil| BalNil{Balance = Nil?}
    BalNil --> CreateFollowUp[Create Follow-up task in Monday]
    CreateFollowUp --> Done1[One Done = Next day]

    %% Second Left Path: Sales / Onboarding needed
    CheckStatus -->|Sales / Onboarding| SalesOnboard{Sales / Onboarding needed?}
    SalesOnboard --> SetContact[Set Contact in Column L (A-I) on BRAID/CRM]
    SetContact --> CreateFollowUp2[Create Follow-up task]
    CreateFollowUp2 --> CreateInvoice[Create Invoice in Monday]
    CreateInvoice --> Done2[One Done = 1-3 Business Days]

    %% Middle Left Path: Return / Duplicate
    CheckStatus -->|Return / Duplicate| ReturnDup{Return / Duplicate in Monday?}
    ReturnDup --> SetStatusL[Set Status in Column (on BRAID/CRM)]
    SetStatusL --> ContactStatusL[Contact Status in CRM]
    ContactStatusL --> UpdateStatus[Update Status in CRM]
    UpdateStatus --> OneDone3[One Done = After 1 day]

    %% Middle Path: Review Needs / Help
    CheckStatus -->|Review Needs / Help| ReviewHelp{Review Needs / Help?}
    ReviewHelp --> CreateTask[Create Follow up task (see notes/emails)]
    CreateTask --> Success[Success Task]
    Success --> Done4[One Done = Remember BRAID/CRM]

    %% Middle Right Path: Status = Success / Handover
    CheckStatus -->|Status = Success| StatusSuccess{Status = Success / Handover?}
    StatusSuccess --> SetInvoiced[Set Invoiced status (Column J) on BRAID/CRM]
    SetInvoiced --> CheckConf{Check 'Time confirmed' field in CRM (Under Admin Section)}
    CheckConf --> StatusSuccess2{Status = Success (on CRM)?}
    StatusSuccess2 --> TaskSuccess[Task Status = For Client / Success team]

    %% Right Path: Other / Misc
    CheckStatus --> StatusOther{Status = Other?}
    StatusOther --> CheckInvoiced{Check Invoiced status (Column J) on BRAID/CRM}
    CheckInvoiced --> No[No]
    CheckInvoiced --> Yes[Yes]
    No --> RequestTask[Request Task]
    Yes --> CheckApprove{Check if approved}
    CheckApprove --> Approved{Approved?}
    Approved -->|Yes| UpdateStatusBRAID[Update Status BRAID = Invoiced to clear down]
    Approved -->|No| RequestInfo[Request task for more info]
    UpdateStatusBRAID --> End([End])

    %% Far Right Path: Status = Inactive
    CheckStatus --> StatusInactive{Status = Inactive?}
    StatusInactive --> UpdateStatusCRM[Update status to inactive in Monday]
    UpdateStatusCRM --> Registered{Registered?}
    Registered -->|Yes| CheckActive[Check if customer has active work with sales/Onboard team]
    Registered -->|No| NoTask[No task]
    CheckActive --> OtherWork{Other active work?}
    OtherWork -->|Yes| UpdateMonday[Update Success Status = Inactive on CRM + Under Customer Section]
    OtherWork -->|No| NoTask2[No task]
    UpdateMonday --> UpdateBRAID[Update Status = Lost in CRM (Under Customer Section)]
    UpdateBRAID --> End
