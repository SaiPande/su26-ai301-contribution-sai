# su26-ai301-contribution-sai
# Contribution 1: Support Syslog Connector (Source & Sink)

**Contribution Number:** 1  
**Student:** Sai Pande 
**Issue:** [https://github.com/apache/seatunnel/issues/2649 ](https://github.com/apache/seatunnel/issues/10753) 
**Status:** Phase I — In Progress

---

## Why I Chose This Issue

I am a computer science student currently in the CodePath AI301 course and this is my first open-source contribution. I was looking through the Apache SeaTunnel umbrella issue (#10753) tracking new connector work and wanted to find something that was unclaimed, accessible to a newcomer, and didn't require any paid or enterprise accounts to test.

Syslog stood out because it is an open, well-documented protocol (RFC 5424/RFC 3164) that can be tested entirely locally with no external accounts or API keys. The scope is also clearly defined — a Source connector that receives Syslog messages over UDP/TCP, and a Sink connector that forwards rows as Syslog messages — making it a great fit for a focused first contribution.

---

## Understanding the Issue

### Problem Description

Apache SeaTunnel does not currently have a Syslog connector. Syslog is a widely used standard protocol for sending log and event messages across systems. Without this connector, users cannot ingest Syslog streams as a data source or forward SeaTunnel rows to a Syslog server as a sink.

### Expected Behavior

- **Source:** SeaTunnel should be able to listen on a configured UDP/TCP port, receive Syslog messages, parse them into rows (fields such as severity, facility, timestamp, host, message), and pass them downstream in a pipeline.
- **Sink:** SeaTunnel should be able to take rows from a pipeline and forward them as formatted Syslog messages to a configured remote Syslog server.

### Current Behavior

No Syslog connector exists in the codebase. Users have no built-in way to connect SeaTunnel pipelines to Syslog infrastructure.

### Affected Components

- A new connector module will need to be created under the `seatunnel-connectors-v2` directory
- Source and Sink implementations following the SeaTunnel connector V2 API
- Configuration schema and documentation
- Unit and integration tests

---

## Reproduction Process

### Environment Setup

[To be filled in as I set up the local dev environment — notes on JDK version, Maven setup, any issues encountered and how I resolved them]

### Steps to Reproduce

1. Check out the `dev` branch of the Apache SeaTunnel repository
2. Search for any existing Syslog connector — none exists
3. Confirm issue #2649 is still open and unclaimed

### Reproduction Evidence

- **Commit showing reproduction:** [To be added]
- **My findings:** The `seatunnel-connectors-v2` directory contains no Syslog module. The umbrella issue #10753 lists Syslog as unclaimed with an existing issue at #2649.

---

## Solution Approach

### Analysis

Syslog is a stateless, message-based protocol. The Source side needs to open a socket (UDP or TCP), listen for incoming messages, parse each message according to RFC 5424 or RFC 3164 format, and emit rows. The Sink side needs to take rows, format them as Syslog messages, and send them to a configured host/port.

### Proposed Solution

Implement a new connector module `connector-syslog` following the SeaTunnel connector V2 pattern used by other connectors in the codebase (e.g., the HTTP-based connectors like Klaviyo and Shopify for structure reference).

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** SeaTunnel needs a Syslog connector that can both receive (Source) and send (Sink) Syslog messages, following the existing connector V2 API patterns.

**Match:** Study the structure of an existing simple connector (e.g., HTTP connector or a similar message-based connector) to understand the required classes, config options, and test patterns.

**Plan:**
1. Create the `connector-syslog` Maven module with the correct directory structure
2. Implement `SyslogSource` — opens a UDP/TCP socket, reads messages, parses fields (priority, version, timestamp, hostname, message) into SeaTunnel rows
3. Implement `SyslogSink` — takes SeaTunnel rows and sends formatted Syslog messages to a configured target
4. Define the config schema (host, port, protocol UDP/TCP, Syslog format RFC version)
5. Write docs and an example config YAML
6. Write unit tests for the parser and sink formatter
7. Write integration tests using a local Syslog listener

**Implement:** [Links to branch/commits to be added as work progresses]

**Review:** Follow the SeaTunnel contribution guide — code style, license headers, test coverage, and PR format

**Evaluate:** Test locally by sending Syslog messages from the terminal using `logger` command (built into Linux/macOS) and verifying rows are emitted correctly by the Source

---

## Testing Strategy

### Unit Tests

- [ ] Test case 1: Parse a valid RFC 5424 Syslog message into correct row fields
- [ ] Test case 2: Parse a valid RFC 3164 (legacy) Syslog message
- [ ] Test case 3: Handle malformed or partial Syslog messages gracefully
- [ ] Test case 4: Sink correctly formats a SeaTunnel row as a Syslog message string

### Integration Tests

- [ ] Source integration: spin up a local UDP listener, send test messages, verify rows
- [ ] Sink integration: spin up a local Syslog receiver, run the sink, verify messages arrive

### Manual Testing

The `logger` command available on Linux and macOS can send Syslog messages to a local port without any additional tools, making local testing simple and free.

---

## Implementation Notes

### Week 1 Progress

Will update soon

### Code Changes

- **Files modified:** Will update soon
- **Key commits:** Will update soon
- **Approach decisions:** Will update soon

---

## Pull Request

**PR Link:** Will update soon

**PR Description:** Will update soon

**Maintainer Feedback:**
- Yet to get

**Status:** Not yet submitted

---

## Learnings & Reflections

[To be filled in as the contribution progresses]

---

## Resources Used

- [Apache SeaTunnel Contribution Guide](https://seatunnel.apache.org/docs/contribution/contribute)
- [RFC 5424 — The Syslog Protocol](https://datatracker.ietf.org/doc/html/rfc5424)
- [RFC 3164 — The BSD Syslog Protocol](https://datatracker.ietf.org/doc/html/rfc3164)
- [SeaTunnel Connector V2 API documentation](https://seatunnel.apache.org/docs/connector-v2/connector-v2-api)
- [GitHub Issue #2649](https://github.com/apache/seatunnel/issues/2649)
- [GitHub Umbrella Issue #10753](https://github.com/apache/seatunnel/issues/10753)
