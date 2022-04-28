---
mir: 1
title: MIR Purpose and Guidelines
author: Igor Rendulic (@igorrendulic)
status: Living
type: Informational
created: 2022-04-28
---

## What is MIR?

MIR stands for Mailio Implementation Rationale. MIR is a design document providing information to Mailio Community or describing new a feature or reasons behind technology selections or it's processes and environment. MIR should provide a concise technical specification of the feature and rationale for the feature. 

## MIR Rationale

MIR is intended for MIR implementers and for collecting community technical and non-technical input on an issue. MIRs are also intended to document the design decisions that have gone into Mailio. 

MIRs are also a good way to track the progress of implementation. 

## MIR Types

There are 3 types of MIRs:
- **Architecture**: Mailio ecosystem architecture and service dependencies 
- **Core**: core service
- **Components**: web and core service components of the Mailio ecosystem
- **Web**: web interface

It is highly recommended that a single MIR contains a single key proposal or new idea. The more focued the MIR, the more successful it tends to be. 

## MIR Workflow

Before you begin writing a formal MIR, you should vet your idea. Ask the Mailio community first if an idea is original to avoid wasting time on something that will be rejected based on prior research. It is thus recommended to open a discussion thread on the [Mailio Discord channel Development](https://discord.gg/hXjFS2zWra) to do this.

Once the idea has been vetted by the Mailio developers, your next responsibility will be to present (by means of an MIR) the idea to the reviewers and all interested parties, invite editors, developers, and the community to give feedback on the MAilio Discord community channels.  Negative community feedback will be taken into consideration and may prevent your MIR from moving past the Draft stage.

### MIR Process 

**Idea** - An idea that is pre-draft. This is not tracked within the MIR Repository.

**Draft** - The first formally tracked stage of an MIR in development. An MIR merged by an MIR Editor into the MIR repository when properly formatted.

**Review** - An MIR Author marks an MIR as ready for and requesting Peer Review.

**Last Call** - This is the final review window for an MIR before moving to `Final`. An MIR editor will assign `Last Call` status and set a review end date (`last-call-deadline`), typically 14 days later.

If this period results in necessary normative changes it will revert the MIR to `Review`.

**Final** - This MIR represents the final standard. A Final MIR exists in a state of finality and should only be updated to correct errata and add non-normative clarifications.

**Stagnant** - Any MIR in `Draft` or `Review` or `Last Call` if inactive for a period of 6 months or greater is moved to `Stagnant`. An MIR may be resurrected from this state by Authors or MIR Editors through moving it back to `Draft` or it's earlier status. If not resurrected, a proposal may stay forever in this status. 
