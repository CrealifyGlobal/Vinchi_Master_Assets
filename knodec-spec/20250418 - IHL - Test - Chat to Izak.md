@prefix knodec: <http://knodec.org/schema#> .
@prefix resume: <http://resume.isaac-leubscher.org#> .
@prefix skos: <http://www.w3.org/2004/02/skos/core#> .
@prefix dct: <http://purl.org/dc/terms/> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

##################################################################
# BLOCK 1: INTERPRETATION GUIDE
##################################################################

resume:Interpretation a knodec:InterpretationGuide ;
    dct:title "Interpretation Guide for Isaac’s Narrative Resume (Knodec Turtle)"@en ;
    dct:description """
    This file encodes a narrative-style resume from Isaac using the Knodec semantic schema. It is structured into three blocks:
    1. Interpretation: This section (you’re reading it) explains how the file is structured and how to reason over it.
    2. Definitions: This declares the classes and properties used in the instance data.
    3. Instance Data: This block contains structured triples representing Isaac’s employment history, skills, and related concepts.

    Key entity: `resume:Isaac` — the subject person.
    Key pattern: Person → Position → Employer → Skills.

    Use `knodec:hasHeldPosition` to find roles Isaac has had.
    Use `knodec:hasEmployer` to find which company each role belongs to.
    Use `knodec:requiresSkill` to explore what skills each role involved.

    This structure allows human and machine readers to follow the semantic intent of the resume clearly.
    """@en ;
    skos:note "Begin at `resume:Isaac`. Follow `knodec:hasHeldPosition` to explore roles, and `knodec:hasEmployer` to identify employers."@en .

##################################################################
# BLOCK 2: DEFINITIONS
##################################################################

resume:DefinitionSet a knodec:DefinitionSet ;
    dct:title "Knodec Vocabulary for Isaac’s Narrative Resume"@en ;
    dct:description "Defines the semantic classes and properties used in this resume instance."@en ;
    
    knodec:usesClass 
        knodec:Person,
        knodec:Organization,
        knodec:Position,
        knodec:Skill,
        skos:Collection,
        knodec:InterpretationGuide,
        knodec:DefinitionSet ;

    knodec:usesProperty 
        knodec:hasHeldPosition,
        knodec:hasEmployer,
        knodec:heldBy,
        knodec:requiresSkill,
        rdfs:label,
        dct:description,
        skos:note,
        skos:member .

##################################################################
# BLOCK 3: INSTANCE DATA
##################################################################

# Person
resume:Isaac a knodec:Person ;
    skos:note "This file captures a narrative summary of Isaac’s career, told informally and interpreted semantically."@en ;
    knodec:hasHeldPosition 
        resume:CollegeRadio_PM,
        resume:PetesSnort_PM,
        resume:GlassCo_PM,
        resume:TestMyShovel_PM .

# Employers
resume:CollegeRadio a knodec:Organization ;
    rdfs:label "College Radio Station"@en ;
    skos:note "Isaac’s first employer out of university."@en .

resume:PetesSnort a knodec:Organization ;
    rdfs:label "Pete’s Snort"@en ;
    skos:note "Second job, company name provided directly in narrative."@en .

resume:GlassCo a knodec:Organization ;
    rdfs:label "Unnamed Glass and Window Company"@en ;
    skos:note "Third employer, focused on glass and window services."@en .

resume:TestMyShovel a knodec:Organization ;
    rdfs:label "Test My Shovel"@en ;
    skos:note "Fourth employer, named in narrative as ‘Test My Shovel’."@en .

# Positions
resume:CollegeRadio_PM a knodec:Position ;
    rdfs:label "Tea Brewer and Coffee Maker"@en ;
    knodec:hasEmployer resume:CollegeRadio ;
    knodec:heldBy resume:Isaac ;
    dct:description "Isaac’s first job after university, where he made tea and coffee for everyone."@en ;
    knodec:requiresSkill resume:Patience,
                         resume:LovingMyWife .

resume:PetesSnort_PM a knodec:Position ;
    rdfs:label "Project Manager"@en ;
    knodec:hasEmployer resume:PetesSnort ;
    knodec:heldBy resume:Isaac ;
    dct:description "Led project management activities at Pete’s Snort."@en ;
    knodec:requiresSkill resume:ProjectManagement,
                         resume:Patience,
                         resume:LovingMyWife .

resume:GlassCo_PM a knodec:Position ;
    rdfs:label "Project Manager"@en ;
    knodec:hasEmployer resume:GlassCo ;
    knodec:heldBy resume:Isaac ;
    dct:description "Handled project management for a company working with glass and windows."@en ;
    knodec:requiresSkill resume:ProjectManagement,
                         resume:Patience,
                         resume:LovingMyWife .

resume:TestMyShovel_PM a knodec:Position ;
    rdfs:label "Project Manager and Handstand Performer"@en ;
    knodec:hasEmployer resume:TestMyShovel ;
    knodec:heldBy resume:Isaac ;
    dct:description "Served as a project manager, and also performed handstands as part of the job."@en ;
    knodec:requiresSkill resume:ProjectManagement,
                         resume:Handstands,
                         resume:Patience,
                         resume:LovingMyWife .

# Skills
resume:ProjectManagement a knodec:Skill ;
    rdfs:label "Project Management"@en .

resume:Patience a knodec:Skill ;
    rdfs:label "Developing Patience"@en .

resume:LovingMyWife a knodec:Skill ;
    rdfs:label "Loving My Wife"@en .

resume:Handstands a knodec:Skill ;
    rdfs:label "Handstands"@en .

# Employer Grouping
resume:PastEmployers a skos:Collection ;
    rdfs:label "Past Employers"@en ;
    skos:note "The four employers Isaac worked for, as described narratively."@en ;
    skos:member 
        resume:CollegeRadio,
        resume:PetesSnort,
        resume:GlassCo,
        resume:TestMyShovel .
