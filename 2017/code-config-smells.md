### Code Configuration Smells

Date: Sat 4th Feb
Time: 17:00
Speaker: Sharma Tushar

* long expressions -- break them up
* missing default cases on case checks -- always handle default case, even if it it's just logging a warning or aborting error
* missing `else`s on `else if`
* inappropriate quotes usage in Chef Ruby
* use 4 digit octal numbers for persmissions, not 3
* violating SRP (single-responsibility principle) for:
    - ansible script
    - chef recipe
    - chef class
    - ALWAYS create ONE module/Chef recipe/ansible script for a single resource (e.g. a DB, one service, SELinux setup, etc.)
    - Allows each to dependent on each other, removes duplication if resource is needed in multiple other resources, etc.

Things to do:

* separate folder for each recipe or module -- keep consistent!
* undense structure of config repo
    - dense means many more modules, which are tightly coupled and there are many inter-dependencies
    - weakened modularity = (Cohesion(A) / Coupling(A))
    - low cohesion = not much relation between things within each component (could be separate components)
    - high coupling = many dependency links between components
    - low cohesion + high coupling = high weakened modularity

### Detecting Configuration Smells

`puppet-lint` for Puppet scripts can detect all the smells above.

Puppeteer is an extension of `puppet-lint` that is also open-source. 

Microsoft Research Paper also available, which has details on code and config smells on 9 million lines of Puppet code.

### Key Takeaways

* treat your config code like you would real code
* keep it modular
* keep it clear
* don't violate SRP
* handle all cases -- use `default` and `case`
* use standards/idioms for orchestration framework used
* always be explicit, don't rely on implicit (e.g. 4 digit octals for permissions, not 3)
    - also use same standards as sysadmins would with bash and shell!
