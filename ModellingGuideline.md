# pumla Modelling Guideline

*Rationale:*

Why do we need a modelling guideline giving us rules to follow?


In order to make re-use of single-source PlantUML models, it is necessary to
follow some modelling rules in order to be easily able to create a tool that
enables the re-use. So these rules and guidelines help us to keep the re-use
tooling *pumla* simple in it's implementation. Without these rules, a lot 
more effort would need to go into the tooling to allow the same 
functionality.


### Rule: Atomicity
In the model files where the re-usable elements are modelled, do 
not model dependencies to other elements. Therefore, do not include other
model files from here.

####*Rationale*

When we want to re-use a model element in a diagram, we need to include
it's *.puml file into our diagram puml file. If another file is included 
by the model element file of the element we want to have on our
diagram, then automatically also the other (dependent) model element of 
the included puml file gets onto our diagram (which we may not want).
So, in order to be able to re-use elements on element level, these files
describing/modelling the elements should be atomic, meaning no other files
are included. Dependencies of this element to other elements are to be
modelled in a separate file, therefore.

####*Example*
#####Wrong:
```PlantUML
file: helloworld.puml
@startuml
!include displayText.puml

component "Hello World" as helloworld {

}

helloworld-->displayText : uses
@enduml
```
Problem here: *displayText* is included in the definition
of the helloWorld component.


#####Right:
```
file: helloworld.puml

@startuml
component "Hello World" as helloworld {
}

file: helloworld_rel.puml

@startuml
' File describing the relations/dependencies 
' of the helloWorld component

!include helloworld.puml
!include displayText.puml

helloworld-->displayText : uses
@enduml
```
### Rule: Mark your model repository
The files that form your re-usable model repository shall be explicitly
marked with the following first line in the puml file:
```
'PUMLAMR
```
This PlantUML comment line exposes the following PlantUML description as
a re-usable asset of the model repository.

####*Rationale*
This explicit marking as a re-usable artefact is needed in order to be
able to separate your re-usable model from other PlantUML diagram files
in your repository. Developers might use other diagrams for e.g.
planning or discussion purposes that should not be part of the re-usable
model. *pumla* will only consider files that are marked like that for the
re-use. There may exist PlantUML diagrams within the repo, where you do 
not want the restrictions of *pumla* and the other files would otherwise
possibly disturb the model-re-use. Still, you can include *pumla* files
into any other *non-pumla* PlantUML diagram file.