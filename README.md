# Kit's Architecture 
## Introduction: 
  
   When migrating to a swift packages in Xcode, there are several challenges that developers may face. This document outlines some of the potential issues and the struggles we faced during the migration.
   
### Documentation

- [Challenges](#Challenges)
- [Dependency conflicts](#Dependency-conflicts)
- [Mitigation](#Mitigation)
- [Learning curve](#Learning-curve)
- [File Structure](#File-Structure)
- [Package Setup](#Package-Setup)


## Challenges

There are few challenges we used to face in kit migration. 
   - Swift packages are struggle to update the latest dependencies and version updating conflicts.
   - May require updating or replacing existing version to work with a new swift packages.
   - Changes to the file structure and build settings may be required, which can be time-consuming and disruptive.
   - Xcode may not support all types of packages, which can require manual integration or workarounds.
   - Smooth development workflow can be maintained with proper planning.
        
## Dependency conflicts

  Configuring new packages can be tricky, especially for projects with many customization needs. Conflicting version update during migration can   lead to errors or unexpected behavior that's hard to resolve if the new package's has different dependencies than the old one.

   - Only consume the kit, and prohibit module usage.
   - Do not directly interact with the module dependency inside the kit's pakages.
     ![cycle-dep](https://user-images.githubusercontent.com/114584154/220047378-72df81e2-7c6d-4904-885f-864ecc1f1611.png)
    
## Minimisation 

   - Before deploying the migration to production, test the changes in a staging environment to ensure that everything is working as expected.
   - Review existing dependencies version changes in Git can help you keep track of changes and provide a way to roll back changes if                necessary.
   - Communicate with other developers when migrating a package, inform them about the migration process, notify them of any necessary changes        they may need to make.
   - Make sure that any dependencies used by the package are up to date and compatible with the new version of Swift.

## Learning curve 

  - Migrating to a new package dependency can be challenging for developers, requiring them to learn accurate naming conventions of dependency,     and versions computability.
  - To overcome these challenges, developers should carefully plan and test the migration process without errors.
  - Steps that can be taken to ensure a smooth transition include resolving dependency conflicts, and unit-testing.

## File Structure

    ├── A_Model                             # Model refers either to a domain model, which represents real state content
    ├── B_Views                             # View is the structure, layout, and appearance of what a user sees on the screen.
    ├── C_ViewModels                        # The view model has been described as a state of the data in the model. (Business Logic)
    ├── D_ViewControllers                   # View Controller is responsible for displaying the data of our iOS application on the screen
    ├── E_Protocols                         # Consists of all the necessary protocols needed by the module
    ├── F_Service                           # The service is used to send HTTP requests or communicate with CoreData, PhotoPicker, and other services.
    ├── H_Resources                         # contains all the resources like image assets, storyboards, and others needed in the module
    ├── <Kit> Module.swift                  # Root file contains all the interface and callback initialization
    └── <kit> ModuleInterface.swift         # File consists of protocols that are needed for Profile Module

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
