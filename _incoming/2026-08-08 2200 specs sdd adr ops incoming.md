The workflow that I want covers everything from feature requests to a living specification and description of design decisions, decision records, operations manuals, basically the whole documentation. 

First off, we start with #incoming. This is a folder in the project. Everything that comes in lands there: every feature request and everything that's unstructured. This is the first part.

These get picked up and discussed, and then I use grill-me and whatever to get alignment. Once I have achieved alignment, this goes into a PRD.

The PRD is translated into beads so the agents can start working on this stuff.
The last bead would contain:
- Pick up the feature request and the PRD and look at the implementation.
- Check any follow-ups regarding SDDs, ADRs, test scenarios, changelogs, roadmaps, ops manuals.
- Put the implementation result into a specification, maybe into a user journey and into other documents to finalize the implementation.
- Look for stuff that needs following up on. Are the current docs still correct?
- Reference the main bead into these resources as well

Schedule for GoLive, note risks, deployments, observation that needs to be done on the feature.
Notify the person that requested it once stuff goes live

Then you can delete the feature request, and I'm pretty sure we also want to delete the PRD.
