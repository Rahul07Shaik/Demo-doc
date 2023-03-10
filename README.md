# Kit's Architecture 
## Introduction: 
  
   When migrating to a new package dependency manager in Xcode, there are several challenges that developers may face. This document outlines some of the potential issues and the struggles we faced during the development of migrating them.
   
### Documentation

- [Challenges](#Challenges)
- [Performance issues](#Performance-issues)
- [Dependency conflicts](#Dependency-conflicts)
- [Mitigation](#Mitigation)
- [Learning curve](#Learning-curve)
- [File Structure](#File-Structure)
- [Package Setup](#Package-Setup)


## Challenges:

   - New package manager compatibility with existing dependencies is a challenge
   - May require updating or replacing existing dependencies to work with a new package manager
   - Compatibility with other project tools and systems should be considered
   - Careful review and testing can minimize disruptions during migration
   - Smooth development workflow can be maintained with proper planning
        
## Dependency conflicts: 

  Configuring the new package manager can be complex and time-consuming, especially if the project has many dependencies or needs customization.   Conflicting dependencies between packages can cause errors or unexpected behavior during migration, which can be difficult to resolve if the new package manager has different conflict resolution rules than the old one.

   - When using a kit's architecture-related dependency, it is important to note that only the kit should be consumed and module usage should be prohibited. This means that any external code using the kit should only access the kit's public interface and not directly interact with its internal modules or implementation details.
     ![cycle-dep](https://user-images.githubusercontent.com/114584154/220047378-72df81e2-7c6d-4904-885f-864ecc1f1611.png)
    
## Mitigation: 

  To ensure a smooth migration to a new package manager, developers should carefully review compatibility with existing dependencies and provide training and resources to the development team. Gradual transition and support during the learning process can reduce frustration and promote a positive attitude.

## Learning curve: 

  - Migrating to a new package dependency manager can be challenging for developers, requiring them to learn new tools, workflows, and terminology.
  - Specific challenges of the learning curve may include familiarity, differences in syntax, and new workflows.
  - To mitigate these challenges, developers should carefully plan and test the migration process.
  - Steps that can be taken to ensure a smooth transition include reviewing compatibility, providing training, automating configuration,             resolving dependency conflicts, and testing performance.

## File Structure

    ????????? A_Model                             # Model refers either to a domain model, which represents real state content
    ????????? B_Views                             # View is the structure, layout, and appearance of what a user sees on the screen.
    ????????? C_ViewModels                        # The view model has been described as a state of the data in the model. (Business Logic)
    ????????? D_ViewControllers                   # View Controller is responsible for displaying the data of our iOS application on the screen
    ????????? E_Protocols                         # Consists of all the necessary protocols needed by the module
    ????????? F_Service                           # The service is used to send HTTP requests or communicate with CoreData, PhotoPicker, and other services.
    ????????? H_Resources                         # contains all the resources like image assets, storyboards, and others needed in the module
    ????????? <Kit> Module.swift                  # Root file contains all the interface and callback initialization
    ????????? <kit> ModuleInterface.swift         # File consists of protocols that are needed for Profile Module

## Package Setup

### Adding the dependency

Adding kits in package dependency:
``` swift
// MARK: - Products 
var products: [Product] = [
    .library(name: "AuthKit", targets: ["AuthKit"]),
```
dependencies:
``` swift 
// MARK: - Dependencies
var dependencies: [PackageDescription.Package.Dependency] = [
    .package(url: "https://github.com/marmelroy/PhoneNumberKit.git", .upToNextMajor(from: .init(3, 3, 3))),
```
Targets:
``` swift 
// MARK: - Targets
var targets: [Target] = [
    .target(
        name: "AuthKit",
        dependencies: [
            "KeychainAccess",
            .product(name: "CommonInterfaceModule", package: "IOS-Common-Interface"),
            .product(name: "NetworkKit", package: "IOS-Base-Kit"),
            .product(name: "AnywhereSerializationKit", package: "IOS-Core-Kit"),
            .product(name: "AnywhereLoggerKit", package: "IOS-Core-Kit")
        ],
        path: "Sources/AuthKit",
        plugins: packagePlugins
    ),
  ]
```
Note: When using a kit's architecture-related dependency, it's crucial to only consume the kit and prohibit module usage. This means that external code should only access the kit's public interface and not interact with its internal modules. This approach promotes better code organization, encapsulation, and easier maintenance.
