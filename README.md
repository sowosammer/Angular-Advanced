# Angular-Advanced
Kurs Angular Advanced

[Schulungsbasislink](https://tinyurl.com/yyogjpjp).

Frage: 
Differenz : Submodule <-> Paket
<link routerlink  ??

## Struktur
Aufteilung

Größe von Paketen 7+/-2 Componenten

ng-packagr macht Pakete (Angular Librarie) in Angular-CLI

Unterteilen der Anwendung in Subprojekte

Libraries und Applicatoin

```ng new lib-prject
```
```cd lib-project
```
```ng g library logger-lib
```ng g application playground-app
```
```ng serve --projekt playground-pp
```ng 

Struktur jetzt eigentlich 
- node_modules
- projects
  -logger-lib
  ..
-src <- gestrichen (wegen existenz in Doku noch beibehalten)

## Publishing to npm Registry

increment version in package.json
  - npm version [patch | minor | major | version ]
 
 
 ng build -- project logger-lib
 
 ### npm packet zur verfügung stellen / holen
 npm publich --registry (? http://localhost:4873)
 
 Namenskonvention: 2 teilige Namen -> erster Teil beginnt mit @; @Datev/myproject
 
 npm install --registry (? http://localhost:4873)
 
 ### Registry für npm
 Global: npm set registry http://localhost:4873  
 
 - .npmrc datei in Root des Projects <- zu empfehlen, dann können andere mit dem gleichen Stand aufsetzen.
 
 Alternative zu Artifactory, Nexus ist Verdaccio (über npm i .. holen)
 
 
 ## Module bzw. Projekte definieren
 
 Unter Pfad projects liegen die Projekte
    - Ein Project wird nach außen dargestellt (quasi Projekt Interface) über Datei public_api.ts. Darin stehen die exports des Projekts.

Früher Service im ap.module.ts registrieren.
Jetzt im Service selbst machen.
@Injectable({
  providedIn: 'root' // steht sofort und überall zur Verfügung 
  // oder     'LazyModule'
)}

### Injectables @Optional
constructor(@optional() private xy: LoggerService

In class namedes**Moduls** (z.B. logger.module.ts) : folgendes static Methode anlegen, wird bereits vom Compiler erstellt. 
Schleifen und ähnlich komplexere Programmierfunktionen sind nicht erlaubt. Meldet aber erst der Prod-Compiler.


```static forRoot(config: LoggerConfig): ModuleWithproviders {
```
```  return 
```    ngModule : LoggerModule,
```    providers:[
```       { provide: LoggerConfig, uneValue: config }
```

in der jeweiligen package.json stehen die INformationen zu Version, und abhängigkeiten zu anderen Paketen/Modulen und deren Versionen.

### Nutzung von Projekten

tsconfig.json löst eigene Projekt referenzen auf. So kann '@myproject/mylib' mit seinem Quellen definiert werden.
Darin entweder auf lokale Pfade, aber auch auf repostiories.

### Verdaccio
Verdaccio (horcht auf port 4873

start mit cmd verdaccio (nachdem er geholt wurde)

ng build --project logger-lib
per npm publish kann in das repository gepushed werden. (hier z.B. Verdaccio)
ggf. muss noch die Version hochgesetzt werden.

## Monorepos
Sollen Nachteile von Libraries vermeiden ohne die Vorteile zu verlieren.
Alles in der Anwendung zusammen. 
Vergleichbar mit Solutions in C#, Workspace in Eclipse.
Vorteile: Gemeinsam in einer Anwendung -> gemeinsame node_modules -> gleiche Versionen

Projectteile können dann aus Monorepo separate z.B. in npm registry auslagern.

## Nx - von Nrwl - erwetiert CLI
Visualisieren Modulstruktur
Kann auch architektur vorgaben (Schichtentrennung) beim übersetzen prüfen.

## Guard - Routerguard
- Services 
- Verhindern eintritt oder auch verlassen (z.B. noch nicht gespeichert)

**Existierende Interfaces:**
- canActivate (Entscheidung auf der Ebene der Componente)
- canActivateChild (Entscheidung auf der Ebene des Parents)
- canLoad (z.B. für LazyLoading interessant)
- canDeactivate<T> (T ~ Componente) (verhindern des verlassens)

Liefern Resultete: boolean, Obervable<boolean> (, Promise<boolean>)
  
Müssen im Module registriert werden. 

# Sonstiges
Quellcode hat gravitation -> Zieht weiteren Quellcode an -> schlecht für Architektur weil Componenten zu groß werden

