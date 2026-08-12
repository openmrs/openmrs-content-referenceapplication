# OpenMRS Reference Application Content

This content package contains the baseline metadata necessary to run a verison of O3. Please note that while
the metadata here is all that's _required_, this does not provide the metadata necessary for every feature to
work. That is because the metadata needed for various things, like the formulary or procedure catalog is
expected to be customized for most sites.

Additional metadata used in the O3 demo environments is contained in the Reference Application Demo content
package: https://github.com/openmrs/openmrs-content-referenceapplication-demo

Generally, the goal is to make the "referenceapplication" content package free from content that an 
implementation might need to customize and to stick to only core content backing specific things. For example,
the O3 Login page requires that the "Login Location" tag be available (and associated with at least one 
location), but locations vary by implementation and even site, so this package just contains the tag.
Similarly, the Immunization feature requires the CIEL Immunization History concept set to structure obs so
this is included but the list of immunizations should not be.

## What's here

The metadata O3 features reference directly, and that a site would not pick for itself:

- **Identity** — the `OpenMRS ID` identifier type, its idgen sequential source and autogeneration option.
- **Encounters and visits** — encounter types, encounter roles and visit types.
- **Access control** — the `Privilege Level: Full`/`High` roles, the `Application: ...` roles that O3 checks,
  and the module privileges they grant.
- **emrapi wiring** — the metadata source, metadata sets and the term mappings O3 resolves by code
  (`emr.primaryIdentifierType`, `emr.admissionEncounterType`, `emr.clinicianEncounterRole`, ...).
- **ADT** — the dispositions, and the ADT concepts they resolve against.
- **Ordering** — order frequencies, dosing units and dispensing statuses, with the CIEL dictionaries backing them.
- **Vitals** — the vital sign concepts, the `Vital signs` convenience set and their reference ranges.
- **Also** — the allergy set, cause of death, stock management concepts, procedure types, relationship types,
  person and visit attribute types, and the encounter print and ID sticker defaults.

Anything a site supplies or replaces stays in the demo package: locations, programs, forms, service queues,
appointment services, the formulary, the lab and diagnosis catalogs, and the address hierarchy.

### This package does not stand up an EMR on its own

Following the boundary above, it ships no locations, so nothing carries the `Login Location` tag. A
distribution built from this package without the demo package has to supply at least one location tagged
`Login Location` and `Visit Location` before anyone can finish logging in.
