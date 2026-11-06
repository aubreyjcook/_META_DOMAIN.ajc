# Problem and Solution Statement

## Problem Overview

### Problem Statement

The problem is that user support requests at the Center for Inquiry reach the technical team through an intermediary: Which is the customer service department, that relays complaints without the technical context needed to resolve them. A user with a problem either calls in or emails customer service; the service agent interprets the complaint but does not understand the underlying technical issues, so they forward whatever the user described, believing it "sounds technical" and therefore actionable by the technical team. In this process, critical information is lost or never gathered: which user is affected, what their configuration or environment is, exactly what they were doing when the problem occurred, and whether the issue is actually a site defect, a misconfiguration on the user's side, or simply incorrect use of the site. All of these specifics are overlooked. There is no single system of record, no structured intake, and the feedback loop between the two teams is not structured to facilitate effective communication.

### Impact of the Problem

The relay-through-a-nontechnical-intermediary model produces a predictable set of failures:

- **Problems with insufficient context** The customer support department does not probe for information adequately, either because they do not understand the systems that the technical problems arise in, or because they do not know what to ask for. Their priority is just to acknowledge the issue and placate the customer, not understand or resolve the problem directly.
- **Escalation based on frustration**
Urgency of a problem is based on how frustrated and agitated the customer acts, not on the merit or impact of their problem. While the customer support team's desire to appease the customer comes from a place of good intent, they lack the objective perspective to provide a resolution.
- **Incomplete tickets:** Technical staff receive vague descriptions with missing context: occasionally missing critical user identity data, no reproduction steps, or no environment details. The technical staff then must spend time chasing information that should have been captured at intake, often by going back through the service agent to the user and back again.
- **Misdiagnosis and wasted effort:** Without confirming details like whether the user is using the site incorrectly, has a bad local configuration, or is encountering a genuine defect, the technical team is mired in inefficient triage efforts. Genuine bugs flood in alongside user error and misconfiguration in the same undifferentiated queue.
- **Blind relay and loss of nuance:** The service agent interprets the problem in terms they understand without technical knowledge, and cannot ask the diagnostic questions the technical team would ask. Each step in the chain degrades the quality of the context, and the original complaint is never verified against what the user actually experienced.
- **No ownership or visibility:** Requests to resolve problems live in emails and phone notes between two departments, without any assignee, no status, and no shared view. A ticket that has stalled may never resurface.
- **No institutional knowledge or metrics:** Resolutions and their root causes are not recorded, so recurring issues like often the same user-side misconfiguration are re-diagnosed from scratch. The analysis of resolutions remains knowledge that only the technical team understands.
- **Poor experience for both users and staff:** Users get no acknowledgment beyond the initial call or email and slow status updates. Service agents don't receive feedback on whether what they forwarded was useful; technical staff absorb the cost of incomplete information.

## Solution Overview

### Proposed Solution

The proposed solution is a purpose-built ticketing support system based on **Next.js** that inserts the necessary structure between the user, the customer service department, and the technical team. Instead of a relay between the two departments, the customer service agents submit requests within a strutured intake form. The form will enforce the information that the technical team agrees it actually needs: the affected user's identity and contact, the site area or feature involved, a description of what the user was doing and what went wrong, the user's environment/configuration details, and an explicit **triage category** distinguishing "possible user error / incorrect use," "possible user misconfiguration," and "suspected site defect." Because the form is structured and mandatory fields are enforced server-side, an incomplete relay cannot silently reach the technical team. Every ticket receives a status from a fixed lifecycle (New → Triaged → In Progress → Resolved → Closed) and an explicit assignee, so stalled or unowned tickets are visible rather than lost in an inbox. A comment thread on each ticket lets the technical team ask the service agent or, via the agent, the user — targeted follow-up questions in one place, and the service agent's relay of the user's answers is preserved in the ticket history. Tickets and their resolution notes are stored in a relational database behind Next.js server actions, giving a single queryable system of record, and an internal dashboard surfaces open tickets by status, assignee, category, and age, plus metrics such as volume by triage category and average time to resolution. This turns the service-to-technical handoff from a blind relay into a structured, auditable workflow while keeping the system minimal enough for a nonprofit to maintain.

### Objectives and Goals

- Provide a single, structured intake form for customer service agents that enforces capture of the user identity, affected feature, description, reproduction steps, environment/configuration details, and triage category.
- Make the user-error vs. misconfiguration vs. defect distinction an explicit, required field so the technical team can triage accurately instead of guessing from a vague relay.
- Prevent incomplete relays from reaching the technical team by validating required fields server-side at submission.
- Make ownership and lifecycle explicit: every ticket has an assignee and a status from a fixed lifecycle, so stalled or unowned tickets are surfaced automatically.
- Give the two teams a shared comment thread per ticket for targeted follow-up questions, preserving the relayed answers in the ticket history.
- Give requesters and service agents acknowledgment and status visibility so neither is left wondering whether a ticket was received or acted on.
- Provide a dashboard with filtering by status, assignee, triage category, and age, plus metrics such as volume by category and average time to resolution.
- Preserve resolution notes and root causes (including recurring user-side misconfigurations) as institutional knowledge.
- Keep the system minimal and maintainable, appropriate to a nonprofit's capacity, rather than adopting a heavyweight ITSM platform.

## Pseudocode

### Pseudocode Overview

The following pseudocode outlines the main logic and structure of the ticketing support system. It covers structured intake by customer service agents, mandatory-field validation, triage categorization, assignment and status transitions, the shared comment thread used for follow-up between the two teams, dashboard aggregation, and the stall-detection check that surfaces unowned or aging tickets. It serves as a blueprint for the actual Next.js implementation (intake form, server actions/API routes, and dashboard).

### Main Logic

```pseudo
Initialize application
Connect to ticket database
Load configuration (triage categories, statuses, stall threshold N days, required fields)
Display service-agent intake form (authenticated customer service staff)
Display technical-team dashboard (authenticated technical staff)

Function submitTicket(formData):
    Validate required fields:
        userIdentity (name and contact)
        affectedFeature (site area or feature)
        description (what the user was doing and what went wrong)
        environmentDetails (browser, device, configuration if relevant)
        triageCategory in ("user_error", "user_misconfiguration", "suspected_defect")
    If validation fails:
        Return errors to service agent, do NOT create ticket
    Create new ticket object with:
        id = generateUniqueId()
        status = "New"
        assignee = null
        triageCategory = formData.triageCategory
        submittedBy = currentServiceAgentId()
        createdAt = currentTimestamp()
        history = [ { event: "created", by: agentId, at: currentTimestamp() } ]
    Save ticket to database
    Send acknowledgment to service agent and (optionally) the affected user with ticket id
    Return ticket reference

Function triageTicket(ticketId, technicalStaffId, confirmedCategory, note):
    Find ticket by ticketId
    Set ticket.assignee = technicalStaffId
    Set ticket.triageCategory = confirmedCategory   // may differ from initial guess
    If ticket.status == "New":
        Set ticket.status = "Triaged"
    Append { event: "triaged", by: staffId, category: confirmedCategory, note: note, at: currentTimestamp() } to ticket.history
    Save ticket
    Notify service agent of triage outcome

Function updateStatus(ticketId, newStatus, note):
    Find ticket by ticketId
    Validate newStatus is a legal transition from ticket.status
    Set ticket.status = newStatus
    Append { event: "status_changed", to: newStatus, note: note, at: currentTimestamp() } to ticket.history
    Save ticket
    Notify service agent and affected user of status change

Function addComment(ticketId, authorId, role, commentText):
    Find ticket by ticketId
    // role is "service_agent" or "technical_staff"
    Append { event: "comment", author: authorId, role: role, text: commentText, at: currentTimestamp() } to ticket.history
    Save ticket
    Notify the other team of the new comment

Function requestFollowUp(ticketId, technicalStaffId, questions):
    Find ticket by ticketId
    Append { event: "follow_up_requested", by: staffId, questions: questions, at: currentTimestamp() } to ticket.history
    Set ticket.status = "Awaiting Info"
    Save ticket
    Notify service agent to relay questions to the user
    // Service agent replies via addComment, preserving the relayed answers

Function detectStalledTickets():
    For each ticket where status not in ("Resolved", "Closed"):
        If ticket.assignee is null AND age(ticket.createdAt) > N days:
            Flag ticket as "unassigned and aging"
        Else if ticket.status == "Awaiting Info" AND age(lastFollowUpRequest(ticket)) > N days:
            Flag ticket as "awaiting info, no response"
        Else if lastStatusChange(ticket) older than N days:
            Flag ticket as "stalled"
    Return list of flagged tickets for dashboard display

Function displayDashboard(filters):
    Query tickets matching filters (status, assignee, triageCategory, age)
    Compute metrics:
        totalOpen = count(tickets where status not in ("Resolved", "Closed"))
        countByTriageCategory = group tickets by triageCategory
        countByStatus = group tickets by status
        incompleteRelayRate = share of tickets that needed follow-up before triage
        avgTimeToResolution = average(resolvedAt - createdAt) over resolved tickets
    Display ticket list with assignee, status, triage category, and age
    Display flagged stalled tickets prominently
    Display aggregated metrics