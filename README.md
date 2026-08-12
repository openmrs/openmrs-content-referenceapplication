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
location), but locations vary by implementation and even site, so this package contains the tag and puts it
on the one location that is not a site's choice: the "Unknown Location" openmrs-core creates in every
database. It defines no facility of its own. Similarly, the Immunization feature requires the CIEL
Immunization History concept set to structure obs so this is included but the list of immunizations should
not be.

## What's here

Metadata that **must be set this way** — an O3 app or module resolves it by uuid, code or name, so an
implementation changing it breaks the feature rather than customising it:

- **Encounters** — the encounter types and encounter roles that O3's apps and the emrapi mappings
  reference by uuid.
- **Access control** — the `Privilege Level: Full`/`High` roles, the `Application: ...` roles that O3
  checks by name, and the module privileges they grant.
- **emrapi wiring** — the metadata source, and the term mappings whose *targets* are fixed
  (`emr.admissionEncounterType`, `emr.visitNoteEncounterType`, `emr.clinicianEncounterRole`, ...).
- **ADT** — the dispositions, and the ADT concepts they resolve against by concept code.
- **Vitals** — the vital sign concepts `esm-patient-vitals-app` resolves by uuid, and the `Vital signs`
  convenience set.
- **Dictionaries a feature depends on structurally** — the allergy set, dosing units, dispensing
  statuses, frequencies, cause of death, stock management concepts.

What an implementation may reasonably want its own version of stays in the demo package, even when O3
needs *something* there: locations, the identifier types and their generator, visit types, order
frequencies, relationship and person attribute types, procedure types, reference ranges, print
defaults, programs, forms, service queues, appointment services, the formulary, the lab and diagnosis
catalogs and the address hierarchy.

That distinction is the point, and it is not the same as "needed to run O3" — most of what is in the
demo package is strictly necessary too. The question is whether a site should be **able** to change it.

### The one bootstrap exception

Locations are the canonical thing an implementation defines for itself, so this package defines **no
facility**. But O3's login page only offers locations tagged `Login Location`, so with the demo package
left out there is nothing to log in *to*. The compromise here is to tag the one location that is not a
site's choice — the `Unknown Location` openmrs-core creates in every database — as a `Login Location`, a
`Visit Location` and a `Queue Location`. No facility is invented and no uuid is created.

`Queue Location` is not optional decoration: `/home` resolves to the Service Queues dashboard, and with
no location carrying that tag `@openmrs/esm-service-queues-app` throws
`Cannot read properties of undefined (reading 'id')`, so the first screen after login is an error page.
Deliberately *not* tagged: `Admission Location` and `Transfer Location` (admitting a patient to "Unknown
Location" is meaningless — a site tags its own wards) and `Facility Location`.

This is a bootstrap concern rather than content, and it arguably belongs in a separate minimal package
alongside an identifier source and a visit type, so that a distribution can boot before an
implementation has authored anything. Until such a package exists it lives here. It is a placeholder,
not a facility: a site is expected to add its own locations, tag those, and retire `Unknown Location` —
anything recorded against it in the meantime stays attached to it.

**Logging in is as far as this package gets you.** Measured on a distribution built from it alone, with
no demo package: login completes and every O3 route renders, but there are no visit types and no idgen
identifier source, so you cannot start a visit and O3's registration has nothing to generate a primary
identifier from. Both are things a site defines for itself, so they stay in the demo package by design —
an implementation is expected to supply them, using that package as the worked example.
