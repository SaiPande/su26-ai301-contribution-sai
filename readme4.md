# Contribution 1: Support Syslog Connector (Source & Sink)

**Contribution Number:** 1  
**Student:** [Your Name]  
**Issue:** https://github.com/apache/seatunnel/issues/2649  
**Status:** Phase IV — In Progress

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

- Java JDK 11 (required by SeaTunnel — JDK 17+ may cause build issues)
- Maven 3.6.3+
- Cloned the fork locally and checked out the `dev` branch
- Ran `mvn install -DskipTests` from the root to verify the build works before making any changes

[To be updated with specific errors encountered and how they were resolved]

### Steps to Reproduce

This is a missing feature issue, not a bug — so reproduction means confirming the gap exists:

1. Clone the fork and check out the `dev` branch
2. Navigate to `seatunnel-connectors-v2/` and search for any Syslog-related module — none exists
3. Check `plugin-mapping.properties` — no Syslog entry present
4. Confirm issue #2649 is still open and unassigned on GitHub
5. **Expected:** A `connector-syslog` module exists under `seatunnel-connectors-v2/`
6. **Actual:** No such module exists — the connector is entirely missing from the codebase

### Reproduction Evidence

- **Working branch:** [To be added — link to your fork branch once created]
- **My findings:** The `seatunnel-connectors-v2/` directory has no Syslog module. The umbrella issue #10753 lists Syslog as unclaimed pointing to #2649. Confirmed with the maintainer on the umbrella issue that this is still open for a PR.

---

## Solution Approach

### Analysis

Syslog is a stateless, message-based protocol. The Source side needs to open a socket (UDP or TCP), listen for incoming messages, parse each message according to RFC 5424 or RFC 3164 format, and emit rows. The Sink side needs to take rows, format them as Syslog messages, and send them to a configured host/port.

### Proposed Solution

Implement a new connector module `connector-syslog` following the SeaTunnel connector V2 pattern used by other connectors in the codebase (e.g., the HTTP-based connectors like Klaviyo and Shopify for structure reference).

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** SeaTunnel has no Syslog connector. The goal for the first PR is to implement the Source side only — a connector that listens on a UDP/TCP port, receives Syslog messages, parses them into SeaTunnel rows, and passes them downstream in a pipeline. This is scoped to Source first as agreed with the maintainer, with Sink as a follow-up.

**Match:** The HTTP-based connectors (Klaviyo, Shopify) in `seatunnel-connectors-v2/` are the closest reference pattern — they follow the Connector V2 API, have a simple request/response or stream model, and include docs + example configs. The Connector V2 API requires implementing `SeaTunnelSource`, `SourceReader`, and `SourceSplitEnumerator` interfaces.

**Plan:**
1. Create a new Maven module `connector-syslog` under `seatunnel-connectors-v2/` mirroring the structure of an existing simple connector
2. Implement `SyslogSource` — implements `SeaTunnelSource`, opens a UDP/TCP server socket on a configured port
3. Implement `SyslogSourceReader` — reads raw incoming Syslog messages from the socket
4. Implement `SyslogMessageParser` — parses RFC 5424 / RFC 3164 formatted messages into SeaTunnel row fields (priority, severity, facility, timestamp, hostname, message body)
5. Define the config options (host, port, protocol UDP/TCP, RFC format version)
6. Register the connector in `plugin-mapping.properties`
7. Write docs and an example `.conf` config file
8. Write unit tests for the parser

**Implement:** [Branch link to be added]

**Review:** Will follow the SeaTunnel CONTRIBUTING.md — Apache license headers on all new files, code style via Checkstyle, commit message format, and PR linked back to both #2649 and #10753

**Evaluate:** Use the Linux/macOS built-in `logger` command to send test Syslog messages to the local port and verify the Source emits correct rows. Run `mvn test` to confirm unit tests pass and no regressions in the build.

---

## Testing Strategy

### Unit Tests

- [ ] Test case 1: Parse a valid RFC 5424 Syslog message into correct row fields
- [ ] Test case 2: Parse a valid RFC 3164 (legacy) Syslog message
- [ ] Test case 3: Handle malformed or partial Syslog messages gracefully
- [ ] Test case 4: Verify config validation rejects missing required fields (host, port)

### Integration Tests

- [ ] Source integration: spin up a local UDP listener, send test Syslog messages using the `logger` command, verify correct rows are emitted
- [ ] Run full `mvn test` suite to confirm no regressions in existing connectors

### Manual Testing

Using the Linux/macOS built-in `logger` command to send real Syslog messages to the local port during development:
```bash
logger -n 127.0.0.1 -P 514 -T "Test message from manual testing"
```
This requires no external tools or accounts and gives immediate feedback on whether the Source is parsing messages correctly.

Modeled test structure on `SyslogMessageParserTest.java` following the pattern used in existing connector unit tests in the codebase.

---

## Implementation Notes

### Week 1 Progress

**What I'm building:**
- Reviewed the SeaTunnel CONTRIBUTING.md and Checkstyle rules before writing any code
- Studied existing simple connectors (HTTP-based Klaviyo/Shopify) to understand the required module structure, interface implementations, and test patterns
- Created the `connector-syslog` Maven module under `seatunnel-connectors-v2/` with the correct directory layout matching other connectors

**Files created so far:**
- `seatunnel-connectors-v2/connector-syslog/pom.xml` — Maven module definition
- `SyslogSource.java` — implements `SeaTunnelSource`, sets up the socket listener
- `SyslogSourceReader.java` — reads raw incoming messages from the UDP/TCP socket
- `SyslogMessageParser.java` — parses RFC 5424 / RFC 3164 messages into SeaTunnel row fields (priority, severity, facility, timestamp, hostname, message body)
- `SyslogSourceConfig.java` — config options (host, port, protocol, RFC format version)

**Challenges faced:**
- Understanding which Connector V2 interfaces are mandatory vs optional took time — studied existing connectors line by line to figure this out
- Maven module registration in the parent `pom.xml` wasn't obvious at first — found the pattern by looking at how other connectors are registered

[To be updated weekly as work progresses]

### Week 2 Progress

[To be filled in]

### Code Changes

- **Working branch:** [Link to your fork branch]
- **Key commits:** [To be added as commits are pushed]
- **Approach decisions:**
  - Scoped first PR to Source only as agreed with maintainer — Sink will be a follow-up PR
  - Supporting both RFC 5424 and RFC 3164 formats in the parser to cover modern and legacy Syslog infrastructure
  - Used Java's built-in `DatagramSocket` (UDP) and `ServerSocket` (TCP) rather than a third-party library to keep dependencies minimal

---

## Pull Request

**PR Link:** [To be added once PR is opened]

**PR Description:**

### What does this PR do?
Adds a new Syslog Source connector to Apache SeaTunnel. The connector listens on a configured UDP or TCP port, receives incoming Syslog messages, parses them according to RFC 5424 or RFC 3164 format, and emits structured SeaTunnel rows with fields for priority, severity, facility, timestamp, hostname, and message body.

### Why was this PR needed?
Apache SeaTunnel had no way to ingest Syslog streams as a data source. Syslog is a standard protocol used widely across servers, routers, firewalls, and cloud systems for remote log forwarding. Issue #2649 tracked this gap. This PR implements the Source side as the first increment, scoped and agreed with the maintainer in umbrella issue #10753.

### What are the relevant issue numbers?
Closes #2649
Related: #10753

### Does this PR meet the acceptance criteria?
- [ ] Tests added for new behavior
- [ ] All tests passing (`mvn test`)
- [ ] Follows SeaTunnel code style (Checkstyle)
- [ ] Apache license headers on all new files
- [ ] No breaking changes to existing connectors
- [ ] Docs and example config included

**Maintainer Feedback:**
- [To be added as feedback comes in]

**Status:** Awaiting review

---

## Learnings & Reflections

### Technical Skills Gained
- How large Java/Maven open-source projects are structured and how to navigate them without getting overwhelmed
- How SeaTunnel's Connector V2 API works — the interface contract between a connector plugin and the core engine
- UDP/TCP socket programming in Java using `DatagramSocket` and `ServerSocket`
- How to parse structured text protocols (RFC 5424/RFC 3164) into typed data fields
- Apache license compliance — adding correct headers, following Checkstyle rules, and understanding why these matter in ASF projects

### Challenges Overcome
- The biggest challenge was figuring out which Connector V2 interfaces are mandatory — the documentation isn't always clear, so studying existing connectors line by line was the most reliable way to understand this
- Maven module registration across parent and child `pom.xml` files was confusing at first but became clear once I traced how existing connectors were registered

### What I'd Do Differently Next Time
- Clone the repo and read 2-3 existing connectors before writing a single line — I'd front-load the codebase exploration even more
- Start with a smaller scope and expand rather than planning both Source and Sink upfront
- Ask the maintainer earlier for a recommended reference connector rather than guessing

---

## Resources Used

- [Apache SeaTunnel Contribution Guide](https://seatunnel.apache.org/docs/contribution/contribute)
- [RFC 5424 — The Syslog Protocol](https://datatracker.ietf.org/doc/html/rfc5424)
- [RFC 3164 — The BSD Syslog Protocol](https://datatracker.ietf.org/doc/html/rfc3164)
- [SeaTunnel Connector V2 API documentation](https://seatunnel.apache.org/docs/connector-v2/connector-v2-api)
- [GitHub Issue #2649](https://github.com/apache/seatunnel/issues/2649)
- [GitHub Umbrella Issue #10753](https://github.com/apache/seatunnel/issues/10753)
- [SeaTunnel Klaviyo Connector (reference pattern)](https://github.com/apache/seatunnel/tree/dev/seatunnel-connectors-v2/connector-http/connector-http-klaviyo)
- [Conventional Commits standard](https://www.conventionalcommits.org)
