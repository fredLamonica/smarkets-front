[![Smarkets Logo](/src/assets/img/readme/smarkets_logo.png)](https://smarkets.com.br)

# mktplace-frontend

<br>

| Enviroment |  From   |                                                                                                          Deployment Status                                                                                                          |                                                                                                          Pipeline Status                                                                                                           |
| :--------: | :-----: | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
|  **HML**   | staging | [![Build Status](https://vsrm.dev.azure.com/smarkets/_apis/public/Release/badge/8363c45e-bd6e-4af3-b058-f286ee56e829/2/2)](https://vsrm.dev.azure.com/smarkets/_apis/public/Release/badge/8363c45e-bd6e-4af3-b058-f286ee56e829/2/2) | [![Build Status](https://dev.azure.com/smarkets/Marketplace/_apis/build/status/Homologa%C3%A7%C3%A3o/CI-FRONT-HML?branchName=staging)](https://dev.azure.com/smarkets/Marketplace/_build/latest?definitionId=4&branchName=staging) |
|  **DMO**   | master  | [![Build Status](https://vsrm.dev.azure.com/smarkets/_apis/public/Release/badge/8363c45e-bd6e-4af3-b058-f286ee56e829/3/7)](https://vsrm.dev.azure.com/smarkets/_apis/public/Release/badge/8363c45e-bd6e-4af3-b058-f286ee56e829/3/7) |   [![Build Status](https://dev.azure.com/smarkets/Marketplace/_apis/build/status/Produ%C3%A7%C3%A3o/CI-FRONT-DMO?branchName=master)](https://dev.azure.com/smarkets/Marketplace/_build/latest?definitionId=11&branchName=master)   |
|  **PRD**   | master  | [![Build Status](https://vsrm.dev.azure.com/smarkets/_apis/public/Release/badge/8363c45e-bd6e-4af3-b058-f286ee56e829/3/5)](https://vsrm.dev.azure.com/smarkets/_apis/public/Release/badge/8363c45e-bd6e-4af3-b058-f286ee56e829/3/5) |   [![Build Status](https://dev.azure.com/smarkets/Marketplace/_apis/build/status/Produ%C3%A7%C3%A3o/CI-FRONT-PRD?branchName=master)](https://dev.azure.com/smarkets/Marketplace/_build/latest?definitionId=9&branchName=master)    |

<br>

## Development environment

[![Node.js v14.19.2](https://shields.smarkets.com.br/static/v1?label=node.js&message=v14.19.2&color=77b063&logo=nodedotjs&logoColor=fff&style=flat)](https://nodejs.org/download/release/v14.19.2/node-v14.19.2-x64.msi)
[![VSCode](https://shields.smarkets.com.br/static/v1?label=vscode&message=latest&color=0082cf&logo=visualstudiocode&logoColor=fff&style=flat)](https://code.visualstudio.com/download)
[![Typescript v3.2.4](https://shields.smarkets.com.br/static/v1?label=typescript&message=v3.2.4&color=3477c1&logo=typescript&logoColor=fff&style=flat)](https://www.npmjs.com/package/typescript/v/3.9.4)
[![Angular v7.2.14](https://shields.smarkets.com.br/static/v1?label=angular&message=v7.2.14&color=e94247&logo=angular&logoColor=fff&style=flat)](https://v7.angular.io/docs)

<!-- Badges Shields: https://shields.io/ (https://shields.smarkets.com.br/) -->

1. Install [**NodeJS**](https://nodejs.org/download/release/v14.19.2/node-v14.19.2-x64.msi "Download NodeJS")
2. Install [**Git for Windows**](https://gitforwindows.org "Download Git for Windows")
3. Install [**VSCode**](https://code.visualstudio.com/download "Download VSCode")
4. Install **Typescript**: `npm install -g typescript@3.2.4`
5. Install **Angular CLI**: `npm install -g @angular/cli@7.3.8`
6. Install **npm packages**: `npm install`
7. Open project folder on **VSCode**
8. Install **Workspace Recommendations** VSCode extensions:
  <br>
   ![C# VSCode extension](/src/assets/img/readme/recommended_extensions_workspace.png)

> When updating the libraries **typescript** and **@angular/cli**, they must be updated globally _(`npm install -g <package_name>@<version>`)_.

<br>

## Guidelines

- **Biblioteca de ícones:**
  - Para os ícones da plataforma utilizamos a biblioteca **Font Awesome** versão **5**.
  - Para busca dos ícones [**clique aqui**](https://fontawesome.com/v5/search?o=r&m=free "Buscar ícones Font Awesome 5 free")

<br>

- **TSLint:** 
  - Configure a **TSLint execution**:
    1. If TSLint execution is blocked, click on the indicator in the status bar: 
    <br>
    ![TSLint Execution - Step 01](/src/assets/img/readme/tslint_execution_01.png)
    2. Click on **Manage Library Execution**: 
    <br>
    ![TSLint Execution - Step 02](/src/assets/img/readme/tslint_execution_02.png)
    3. Click on **Always enable workspace library execution**: 
    <br>
    ![TSLint Execution - Step 03](/src/assets/img/readme/tslint_execution_03.png)
    <br>
  - On save code TSLint will fix all auto fix
  - Other warnings should be corrected manually and those that cannot be resolved due to usage impacts should be ignored

<br>

- **Stream subscriptions:**
  - Use the **`Unsubscriber` base component class** to unsubscribe all streams of a **component** when it is **destroyed**:

    ```ts
    import { Component, OnInit } from '@angular/core';
    import { takeUntil } from 'rxjs/operators';

    @Component({
      selector: 'app-foo',
      templateUrl: './foo.component.html',
      styleUrls: ['./foo.component.scss']
    })
    export class FooComponent extends Unsubscriber implements OnInit {

      bar: number;

      constructor(private fooService: FooService) {
        super();
      }

      ngOnInit() {
      }

      getDefaultBar(): number {
        this.fooService.getDefaultBar().pipe(
          takeUntil(this.unsubscribe))
          .subscribe((bar) => {
            this.bar = this.baz ? bar : bar + 1;
          });
      }

    }
    ```

  - If the component implements **OnDestroy** interface then the **ngOnDestroy** method must invoke the base method with `base.ngOnDestroy()`:
    ```ts
    import { Component, OnInit, OnDestroy } from '@angular/core';

    @Component({
      selector: 'app-foo',
      templateUrl: './foo.component.html',
      styleUrls: ['./foo.component.scss']
    })
    export class FooComponent extends Unsubscriber implements OnInit, OnDestroy {

      ...

      ngOnDestroy(): void {
        
        // some code...

        super.ngOnDestroy();
      }

    }
    ```


<br>

- **Class contructor:**
  - Always create the constructor in classes that build a partial object to minimize impacts on evolutions:

    ```ts
    export class Foo {

      bar: number;
      baz: string;

      constructor(init?: Partial<Foo>) {
        Object.assign(this, init);
      }

    }
    ```

<br>

## Run .NET Core (3.x) on VSCode

1. Install [**C#**](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp "Install C# VSCode extension") VSCode extension
<br>
![C# VSCode extension](/src/assets/img/readme/csharp_vscode_extension.png)
2. Install [**.NET SDK (3.x)**](https://dotnet.microsoft.com/en-us/download/dotnet/thank-you/sdk-3.1.416-windows-x64-installer "Download .NET SDK (3.x)")
3. Load projects:

   - [**Problem link**](https://github.com/OmniSharp/omnisharp-roslyn/issues/1311#issuecomment-428361674 "Problem thread")
   - **Solution:**
     1. Install [**Visual Studio Build Tools 2019**](https://aka.ms/vs/16/release/vs_buildtools.exe "Visual Studio Build Tools 2019"):
         - Check this options:
            - on tab **Componentes individuais:**
              - inside **.NET**
                - [x] Runtime do .NET 5.0
                - [x] Runtime do .NET Core 3.1 (LTS)
                - [x] SDK do .NET
              - inside **Ferramentas de código**
                - [x] Os destinos e as tarefas de compilação do NuGet
            - on tab **Pacotes de idiomas:**
              - [x] check **Inglês**
              - [ ] uncheck **Português (Brasil)**
     2. Open project folder on VSCode
     3. Open an `.cs` file
     4. Open tab **Run and Debug** *(Ctrl+Shift+D)*
     5. Click on **Generate C# Assets for Build and Debug**
     6. Select the project **Marketplace.WebApi**
     7. The **launch.json** file must have been created inside the **.vscode** folder referencing **Marketplace.WebApi.dll**
     8. In the **launch.json** file comment out the **serverReadyAction** entry
     9. Now run the project by pressing **F5**

<br>

> &nbsp;
> Error **Response status code does not indicate success: 401 (Unauthorized)** when restoring **NuGet** packages:
> 1. Install [**Visual Studio Community**](https://aka.ms/vs/17/release/vs_community.exe "Download Visual Studio Community")
> 2. Uninstall **Visual Studio Community** :)
> 3. **Restart** the machine
> 4. **Delete** project folder
> 4. **Clone** repository again
> 4. **Delete** the folder `C:\Program Files\dotnet\sdk\6.x`
> &nbsp;

<br>

# This project was generated with [Angular CLI](https://github.com/angular/angular-cli)

## Development server

Run `ng serve` for a dev server. Navigate to `http://localhost:4200/`. The app will automatically reload if you change any of the source files.

## Code scaffolding

Run `ng generate component component-name` to generate a new component. You can also use `ng generate directive|pipe|service|class|module`.

## Build

Run `ng build` to build the project. The build artifacts will be stored in the `dist/` directory. Use the `-prod` flag for a production build.

<!-- 
## Running unit tests

Run `ng test` to execute the unit tests via [Karma](https://karma-runner.github.io).

## Running end-to-end tests

Run `ng e2e` to execute the end-to-end tests via [Protractor](http://www.protractortest.org/).
Before running the tests make sure you are serving the app via `ng serve`.
-->

## Further help

To get more help on the Angular CLI use `ng help` or go check out the [Angular CLI README](https://github.com/angular/angular-cli/blob/master/README.md).
