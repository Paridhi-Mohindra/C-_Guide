# C# Backend Coding Conventions

## Linting Standards
1. The linting configuration exists in `<service-name>/.editorconfig` file in service each repo.
   ```sh
    [*]
    charset = utf-8
    end_of_line = lf
    trim_trailing_whitespace = false
    insert_final_newline = false
    indent_style = space
    indent_size = 4

    # Microsoft .NET properties
    csharp_new_line_before_members_in_object_initializers = false
    csharp_preferred_modifier_order = public, private, protected, internal, new, static, abstract, virtual, sealed, readonly, override, extern, unsafe, volatile, async:suggestion
    csharp_space_after_cast = true
    csharp_style_var_for_built_in_types = false:suggestion
    dotnet_naming_rule.unity_serialized_field_rule.severity = warning
    dotnet_naming_rule.unity_serialized_field_rule.style = lower_camel_case_style
    dotnet_naming_rule.unity_serialized_field_rule.symbols = unity_serialized_field_symbols
    dotnet_naming_style.lower_camel_case_style.capitalization = camel_case
    dotnet_naming_symbols.unity_serialized_field_symbols.applicable_accessibilities = *
    dotnet_naming_symbols.unity_serialized_field_symbols.applicable_kinds =
    dotnet_style_parentheses_in_arithmetic_binary_operators = never_if_unnecessary:none
    dotnet_style_parentheses_in_other_binary_operators = never_if_unnecessary:none
    dotnet_style_parentheses_in_relational_binary_operators = never_if_unnecessary:none
    dotnet_style_predefined_type_for_locals_parameters_members = true:suggestion
    dotnet_style_predefined_type_for_member_access = true:suggestion
    dotnet_style_qualification_for_event = false:suggestion
    dotnet_style_qualification_for_field = false:suggestion
    dotnet_style_qualification_for_method = false:suggestion
    dotnet_style_qualification_for_property = false:suggestion
    dotnet_style_require_accessibility_modifiers = for_non_interface_members:suggestion

    # ReSharper properties
    resharper_for_other_types = use_explicit_type
    resharper_for_simple_types = use_explicit_type
    resharper_space_within_single_line_array_initializer_braces = false

    [{*.har,*.inputactions,*.jsb2,*.jsb3,*.json,.babelrc,.eslintrc,.stylelintrc,bowerrc,jest.config}]
    indent_style = space
    indent_size = 2

    [*.{appxmanifest,asax,ascx,aspx,axaml,build,cg,cginc,compute,cs,cshtml,dtd,fs,fsi,fsscript,fsx,hlsl,hlsli,hlslinc,master,ml,mli,nuspec,paml,razor,resw,resx,shader,skin,usf,ush,vb,xaml,xamlx,xoml,xsd}]
    indent_style = space
    indent_size = 4
    tab_width = 4

    [*.cs]
    # CS8618: Non nullable field _name is not initialized. Consider declare the field as nullable type
    dotnet_diagnostic.CS8618.severity = none
    csharp_new_line_before_open_brace = methods, properties, control_blocks, types
    csharp_insert_final_newline = true
    csharp_brace_style = next_line
    csharp_prefer_braces = true
   ```
2. All rules for braces, indentation,semi-colons, whitespaces etc are specified in the above configuration file.
3. If the IDE in use is Rider by JetBrains, right click on the ServiceName directory and select `Reformat and code cleanup` option as shown below.
![reformat-and-cleanup-code](../assets/reformat-and-cleanup-code.png)

## Naming conventions

1. Declare one variable in one line per variable declaration.
2. Use PascalCase for file, classes and function names.
3. Use camelCase for variable names inside functions.
4. Private variables inside each class should be declared as `readonly` and the names should start with `_` underscore followed by camelCasing. E.g.:- `    private readonly ICustomerRepository _customerRepository;  `
5. All constants should be declared in `Constants/StaticValues.cs` file following PascalCase.
6. Constant dictionaries are declared in the following manner:-
   ```c#
       public static readonly Dictionary<string, UserRoleEnum> UserTypeRoleMap = new()
    {
        {"1", UserRoleEnum.Dealer},
        {"2", UserRoleEnum.Expert},
        {"3", UserRoleEnum.Dealer},
        {"Customer", UserRoleEnum.Customer}
    };
   ```
7. In cases where enums are being stored in the databases, assign a number to each value in the enum to that if an intermediate step is added/order changes, the data in the DB is consistent. E.g:-
    ```c#
    public enum JobInformationStatusEnum
    {
        DiagnosisStarted = 0,
        DiagnosisInProgress = 1,
        DiagnosisSubmitted = 2,
        DiagnosisPaid = 3
    }
    ```
8. Test descriptions should follow the following format:-
   ```c#
   [Fact(DisplayName =
        "<ClassName>: <FunctonName> - Should <functionality>.")]
   ```
   E.g.:-
    ```c#
    [Fact(DisplayName =
        "CustomerService: GetCustomer - Should be able to get customer details with profile photo url.")]
    ```

## Cognitive complexity
1. Make the function as modular as possible.
2. Make sure the function follows SRP - Single Responsibility Principle.
3. The Business Logic reside in the `Services` folder.
4. The database communications reside in `Repositories` folder.
5. All custom validations for request bodies reside in `Validations` folder.


## Package/Library import
1. Use the LTS release for any public package which is available at the nuget org feed in this index: `https://api.nuget.org/v3/index.json`.
2. Any functionality common to all the micro-services reside in the `pod-commons` library, which is available in the feed:  `https://sportscrm.pkgs.visualstudio.com/75d8e0cd-c6f9-4c3f-a2c8-f4438b0aece4/_packaging/pod-commons/nuget/v3/index.json`.
3. These configurations are specified in `<ServiceName>.API/nuget.config` in each micro-service.
    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <configuration>
        <packageSources>
            <clear/>
            <add key="pod-commons"
                value="https://sportscrm.pkgs.visualstudio.com/75d8e0cd-c6f9-4c3f-a2c8-f4438b0aece4/_packaging/pod-commons/nuget/v3/index.json"/>
            <add key="nuget.org" value="https://api.nuget.org/v3/index.json" protocolVersion="3"/>
        </packageSources>
    </configuration>
    ```
4. You can check out the latest version of the pod-commons library in use in the `Artifacts` section in Azure DevOps.
   https://sportscrm.visualstudio.com/Pool%20on%20Demand/_artifacts/feed/pod-commons


## General standards
1. Use slashes `//` for comments.
2. Use
    ```c#
    /*
    .....
    */
    ```
    for multi-line comments.
3. Specify package name at the top
    E.g.:- `using Serilog;`
4. Specify namespace after all the library imports.
    E.g.:- `namespace PodCommonsLibrary.Core.Middleware;`
5. Specify resources like html files in csproj.
    E.g.:- In Notification Service:-
    ```xml
    <ItemGroup>
        <EmbeddedResource Include="Reources\File1.html"/>
        <EmbeddedResource Include="Reources\File2.html"/>
    </ItemGroup>
    ```
    In order to read the file, use the following:-
    ```c#
     Assembly assembly = Assembly.GetExecutingAssembly();
     Stream stream = assembly.GetManifestResourceStream("ServiceName.API.Resources.File1.html")!;
    ```
6. Write unit test cases for all new functions added in Services, Controllers and Clients.

## Exception Handling
1. All the exceptions are handled globally in pod-commons library using `GlobalExceptionHandlerMiddleware`.
2. The different types of exceptions are in `PodCommonsLibrary.Core/Exceptions` folder.
3. There is a switch case which maps which HTTP status code and message should be given for a particular exception.
4. Whenever a new exception is to be added, add the exception class to the `PodCommonsLibrary.Core/Exceptions` folder and add a new case in the switch statement in `GlobalExceptionHandlerMiddleware` to handle the exception.

## Logging Standards
1. Use structured logging to provide a well-defined format for the log entries, so that it can be easily processed by analytics tool in the future. [Serilog](https://serilog.net/) is used in the BE microservices. 
2. Define a specific format for all the logs to follow. `appsettings.json` has the template.
   ```
   [{MachineName} | {ApplicationName} | {Timestamp:o} | {CorrelationId} | {Level:u3}] {Message:lj}{NewLine}{Exception}`
   ```
   Example - 
   ```
   [BEBR-198745-C02ZT1EBMD6V | Job Service | 2022-08-12T16:59:06.1993330+05:30 | f3fc4ece-0bdc-4bbd-bbc7-8be32742c5c1 | INF] Request finished HTTP/1.1 GET https://localhost:5008/swagger/v1/swagger.json - - - 200 - application/json;charset=utf-8 419.6412ms
   ```
2. Logger must be configurable to enable and disable specific level of logs based on environment.
3. All the parameters of the API requests must be logged for visibility. The complete details of the request is stored in `ApiLogEntries` table. Serilog is globally configured in `Program.cs` to log all the requests and responses.
   ```
   [BEBR-198745-C02ZT1EBMD6V | Job Service | 2022-08-12T18:22:42.3673380+05:30 | deb687e1-d7c0-48df-8a47-a55c4b7a62ed | INF] Request starting HTTP/1.1 GET https://localhost:5008/api/customers/expert-summaries 
   [BEBR-198745-C02ZT1EBMD6V | Job Service | 2022-08-12T18:22:43.3164350+05:30 | deb687e1-d7c0-48df-8a47-a55c4b7a62ed | INF] Request finished HTTP/1.1 GET https://localhost:5008/api/customers/expert-summaries - - - 200 - application/json;+charset=utf-8 949.1443ms
   ```
5. Do not include sensitive information in the logs.
6. Use correct log level
   1. INFO - Informational messages that do not indicate any fault or error. 
   2. WARN - Indicates that there is a potential problem, but with no user experience impact. 
   3. ERROR - Indicates a serious problem, with some user experience impact. 
   4. FATAL - Indicates fatal errors, user experience is majorly impacted. 
   5. DEBUG - Used for debugging. The messaging targets specifically the app’s developers.

## Audit
1. All the DB Models have their corresponding Audit Models. 
2. While creating New DB model, we always create an Audit Model with properties we need to audit/track.
3. These Audit Models inherit BaseAudit to provide us properties like AuditAction, AuditDate and UserName.
4. DbContext file needs to be updated with new Model and AuditModel mappings.
5. We have configured audit in constructor of DbContext file which further uses the Audit library in entity framework core.
    
## PR Guidelines
Development and Git branching strategy for microservice repos:
- Create feature or fix branches such as `feature/<name-of-feature>` or `fix/<name-of-bug>`
        e.g. `feature/customer-login`
- Commit and push code to feature branches and raise Pull Request(PR) to develop.
- This triggers the PR policies such as security scan for packages, dotnet build and unit tests.
- Every PR has a description format specified in `.azuredevops/pull_request_template.md` to describe the modification and specify the change type(E.g.:- feat, fix, style, refactor, test,docs, chore).
- Link Azure Devops story ticket to the PR as a work item.
- The Backend lead has to be a required reviewer.
- All comments must be resolved on the PR.
- Once the PR is merged to develop, the code is built into a docker image and pushed to ACR. The build id is used for tagging the image.
- This will auto trigger the deployment for the development environment.
- Approvals are required for the other environments. And the CD pipeline is Dev -> QA -> Staging -> Production.
