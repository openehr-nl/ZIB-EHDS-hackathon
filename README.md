# ZIB-EHDS-hackathon
Repo containing materials for a hackathon to link BGZ FHIR recource to openEHR to EHDS FHIR.

# Outline
This project aims to output a FHIR resources (allery intolerance only for now) conforming to the EHDS EPS FHIR profile. Based on a BGLZ input resource. (supposedly produced by Nedap Ons, used an example from simplifier for now: https://simplifier.net/packages/nictiz.fhir.nl.stu3.zib2017/2.3.1/files/2980360).

# prerequisites
- docker running locally
- postman with network access to the docker network (e.g. running locally)

# Steps to replicate
1. Clone this repo to your local filesystem
2. Get a firely license at https://fire.ly/firely-server-trial
   Once you have it, place it in the licenses/ folder inside infrastructure/ subfolder and name it firely-license.json.
3. Get an openFHIR license (only required for terminology mappings) at https://open-fhir.com/#access
   Once you have it, place it in the licenses/ folder inside infrastructure/ subfolder and name it openfhir-license.json.
4. `docker compose -f infrastructure/docker-compose.yml up -d`
5. import collection from /files in postman
6. open the `openEHR template DataHub Allergy to EHRBase` call and set the ehrbase password `ehrbase_restricted` by default
7. post the template (in the body) to ehrBASE by sending the call
8. create the patient in Firely by sending the `Create Patient in Firely` call
9. send `Push Bundle STU3 for AllergyIntolerance into Firely`
10. update `Patient/XXXX` in `Push Bundle STU3 for AllergyIntolerance into Firely` (line 89) body with the value from the response of the previous call
11. send the `Push Bundle STU3 for AllergyIntolerance into Firely` call
12. update the url in `Get Patient EHDS Summary Copy from Firely` with the identifier from step 9
13. send `Get Patient EHDS Summary Copy from Firely`
14. :tada:
    
# Limitations 
We used an example resource from nictiz, not a resource actually produced by e.g. Nedap Ons.

# Alternative setups
We used Firely for convenience the setup should work with HAPI FHIR as well, for users prefering an open source setup.
We used openFHIR locally, we could have used openFHIR sandbox to save on setup time.

# Future steps
Expand the setup for the rest of the BGLZ/EHDS, dependent on FHIR-Connect mappings in https://github.com/openehr-nl/ZIBs-on-openEHR and https://github.com/freshehrteam/EHDS/tree/eps_fhirconnect_mappings-terminology/Mappings/fhirconnect .
Any additional mappings need to be added to /infrastructure to be loaded by openFHIR on startup, or added to openFHIR using the approriate api call.
Test this readme with a user without our context.
Expand the readme with a story section outlining the value of this setup in a broader context.
Add compatibility for FHIR stu3.
