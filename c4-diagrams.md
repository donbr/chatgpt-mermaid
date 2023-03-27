# C4 Sample Diagrams

- thanks to Luke Merrett for these examples:  https://lukemerrett.com/building-c4-diagrams-in-mermaid/
- experimental diagrams from the mermaid site:  https://mermaid.js.org/syntax/c4c.html
  - used only for Level 4 diagrams

## Level 1: System Context Diagram

```mermaid
flowchart TB

subgraph personalBankingCustomer[Personal Banking Customer]
    h1[-Person-]:::type
    d1[A customer of the bank, with \n personal bank accounts]:::description
end
personalBankingCustomer:::person

subgraph internetBankingSystem[Internet Banking System]
    h2[-Software System-]:::type
    d2[Allows customers to view \n information about their bank \n banks, and make payments]:::description
end
internetBankingSystem:::internalSystem

subgraph mainframeBankingSystem[Mainfram Banking System]
    h3[-Software System-]:::type
    d3[Stores all of the core banking \n information about customers, \n accounts, transactions etc]:::description
end
mainframeBankingSystem:::externalSystem

subgraph emailSystem[Email System]
    h4[-Software System-]:::type
    d4[The internal Microsoft Exchange \n email system]:::description
end
emailSystem:::externalSystem

personalBankingCustomer--Views account \n balances, and \n makes payments \n using-->internetBankingSystem
internetBankingSystem--Gets accounts \n information from, \n and makes \n payments using-->mainframeBankingSystem
internetBankingSystem--Sends emails using--> emailSystem
emailSystem--Sends emails to-->personalBankingCustomer

%% Element type definitions

classDef person fill:#08427b
classDef internalSystem fill:#1168bd
classDef externalSystem fill:#999999

classDef type stroke-width:0px, color:#fff, fill:transparent, font-size:12px
classDef description stroke-width:0px, color:#fff, fill:transparent, font-size:13px
```


## Level 2: Container Diagram

```mermaid
flowchart TB

subgraph personalBankingCustomer[Personal Banking Customer]
    h1[-Person-]:::type
    d1[A customer of the bank, with \n personal bank accounts]:::description
end
personalBankingCustomer:::person

personalBankingCustomer--Visits bigbank.com using HTTPS-->webApplication
personalBankingCustomer--Views account \n balancs, and \n makes payments \n using-->singlePageApplication
personalBankingCustomer--Views account \n balancs, and \n makes payments \n using-->mobileApp

subgraph internetBankingSystem[Internet Banking System]
    subgraph webApplication[Web Application]
        direction LR
        h2[Container: Java and Spring MVC]:::type
        d2[Delivers the static content and the \n Internet banking single page \n application]:::description
    end
    webApplication:::internalContainer

    subgraph singlePageApplication[Single Page Application]
        direction LR
        h3[Container: JavaScript and Angular]:::type
        d3[Provides all of the Internet banking \n fuctionality to customers via their \n web browser]:::description
    end
    singlePageApplication:::internalContainer

    subgraph mobileApp[Mobile App]
        direction LR
        h4[Container: Xamarin]:::type
        d4[Provides a limited subset of the \n Internet banking functionality to \n customers via their mobile device]:::description
    end
    mobileApp:::internalContainer

    subgraph apiApplication[API Application]
        direction LR
        h5[Container: Java and Spring MVC]:::type
        d5[Provides Internet banking \n functionality via a JSON/HTTP API]:::description
    end
    apiApplication:::internalContainer

    subgraph database[Database]
        direction LR
        h6[Container: Oracle Database Schema]:::type
        d6[Stores user registration information, \n hashed authentication credentials, \n access logs, etc]:::description
    end
    database:::internalContainer

    webApplication--Delivers to the \n customer's web \n browser-->singlePageApplication
    singlePageApplication--Makes API calls to-->apiApplication
    mobileApp--Makes API calls to-->apiApplication
    apiApplication--Reads from and \n writes to-->database
end

apiApplication--Sends emails using-->emailSystem
apiApplication--Makes API calls to-->mainframeBankingSystem

subgraph mainframeBankingSystem[Mainfram Banking System]
    h98[-Software System-]:::type
    d98[Stores all of the core banking \n information about customers, \n accounts, transactions etc]:::description
end
mainframeBankingSystem:::externalSystem

subgraph emailSystem[Email System]
    h99[-Software System-]:::type
    d99[The internal Microsoft Exchange \n email system]:::description
end
emailSystem:::externalSystem

emailSystem--Sends emails to-->personalBankingCustomer

%% Element type definitions

classDef person fill:#08427b
classDef internalContainer fill:#1168bd
classDef externalSystem fill:#999999

classDef type stroke-width:0px, color:#fff, fill:transparent, font-size:12px
classDef description stroke-width:0px, color:#fff, fill:transparent, font-size:13px
```

## Level 3: Component Diagram

```mermaid
flowchart TB

subgraph singlePageApplication[Single Page Application]
    direction LR
    h3[Container: JavaScript and Angular]:::type
    d3[Provides all of the Internet banking\nfuctionality to customers via their\nweb browser]:::description
end
singlePageApplication:::internalContainer

subgraph mobileApp[Mobile App]
    direction LR
    h4[Container: Xamarin]:::type
    d4[Provides a limited subset of the\nInternet banking functionality to\ncustomers via their mobile device]:::description
end
mobileApp:::internalContainer

subgraph database[Database]
    direction LR
    h6[Container: Oracle Database Schema]:::type
    d6[Stores user registration information, \n hashed authentication credentials, \n access logs, etc]:::description
end
database:::internalContainer

subgraph mainframeBankingSystem[Mainfram Banking System]
    h98[-Software System-]:::type
    d98[Stores all of the core banking \n information about customers, \n accounts, transactions etc]:::description
end
mainframeBankingSystem:::externalSystem

subgraph emailSystem[Email System]
    h99[-Software System-]:::type
    d99[The internal Microsoft Exchange \n email system]:::description
end
emailSystem:::externalSystem

singlePageApplication--Make API calls to-->signInController
singlePageApplication--Make API calls to-->resetPasswordController
singlePageApplication--Make API calls to-->accountsSummaryController
mobileApp--Make API calls to-->signInController
mobileApp--Make API calls to-->resetPasswordController
mobileApp--Make API calls to-->accountsSummaryController

subgraph apiApplication[API Application]
    subgraph signInController[Sign In Controller]
        direction LR
        h10[Component: Spring MVC Rest Controller]:::type
        d10[Allows users to sign in to the Internet \n Banking System]:::description
    end
    signInController:::internalComponent

    subgraph resetPasswordController[Reset Password Controller]
        direction LR
        h20[Component: Spring MVC Rest Controller]:::type
        d20[Allows users to reset their passwords \n with a single use URL]:::description
    end
    resetPasswordController:::internalComponent

    subgraph accountsSummaryController[Accounts Summary Controller]
        direction LR
        h30[Component: Spring MVC Rest Controller]:::type
        d30[Provides customers with a summary \n of their bank accounts]:::description
    end
    accountsSummaryController:::internalComponent

    subgraph securityComponent[Security Component]
        direction LR
        h40[Component: Spring Bean]:::type
        d40[Provides functionality related to \n signing in, changing passwords, etc]:::description
    end
    securityComponent:::internalComponent

    subgraph emailComponent[Email Component]
        direction LR
        h50[Component: Spring Bean]:::type
        d50[Sends emails to users]:::description
    end
    emailComponent:::internalComponent

    subgraph mainframeBankingSystemFacade[Mainframe Banking System Facade]
        direction LR
        h60[Component: Spring Bean]:::type
        d60[A facade onto the mainframe \n banking system]:::description
    end
    mainframeBankingSystemFacade:::internalComponent

    signInController--Uses-->securityComponent
    resetPasswordController--Uses-->securityComponent
    resetPasswordController--Uses-->emailComponent
    accountsSummaryController--Uses-->mainframeBankingSystemFacade
end

securityComponent--Reads from and \n writes to-->database
emailComponent--Sends email using-->emailSystem
mainframeBankingSystemFacade--Uses-->mainframeBankingSystem

%% Element type definitions

classDef person fill:#08427b
classDef internalContainer fill:#1168bd
classDef internalComponent fill:#4b9bea
classDef externalSystem fill:#999999

classDef type stroke-width:0px, color:#fff, fill:transparent, font-size:12px
classDef description stroke-width:0px, color:#fff, fill:transparent, font-size:13px
```

## Level 4: Code Diagram (Dynamic)

```mermaid
    C4Dynamic
    title Dynamic diagram for Internet Banking System - API Application

    ContainerDb(c4, "Database", "Relational Database Schema", "Stores user registration information, hashed authentication credentials, access logs, etc.")
    Container(c1, "Single-Page Application", "JavaScript and Angular", "Provides all of the Internet banking functionality to customers via their web browser.")
    Container_Boundary(b, "API Application") {
      Component(c3, "Security Component", "Spring Bean", "Provides functionality Related to signing in, changing passwords, etc.")
      Component(c2, "Sign In Controller", "Spring MVC Rest Controller", "Allows users to sign in to the Internet Banking System.")
    }
    Rel(c1, c2, "Submits credentials to", "JSON/HTTPS")
    Rel(c2, c3, "Calls isAuthenticated() on")
    Rel(c3, c4, "select * from users where username = ?", "JDBC")

    UpdateRelStyle(c1, c2, $textColor="red", $offsetY="-40")
    UpdateRelStyle(c2, c3, $textColor="red", $offsetX="-40", $offsetY="60")
    UpdateRelStyle(c3, c4, $textColor="red", $offsetY="-40", $offsetX="10")

```