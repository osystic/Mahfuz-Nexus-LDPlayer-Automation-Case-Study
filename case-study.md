# Case Study: Multi-LDPlayer Automation Control Center

## Problem

A desktop operator needed to coordinate multiple Android emulator instances from one Windows application while keeping task ownership, browser state, proxy configuration, and failures isolated per emulator.

Browser first-run states, Android permission prompts, UI timing, malformed persisted state, and third-party page variations could cause a worker to fail before the intended task had actually started.

## Constraints

- Windows desktop application
- Multiple LDPlayer instances
- Browser-driven Android workflow
- Per-instance proxy support
- Persistent task queue
- Bounded retries and recoverable lifecycle states
- No automatic OTP entry or password change
- No assumption that a running browser process is actually ready

## Architecture decision

The system uses one worker per configured emulator while retaining shared coordination only where Android UI tooling can conflict. Browser readiness is established before task claim, so a bootstrap problem cannot consume a real task.

## R5 lifecycle correction

The corrective phase addressed a failure loop in which a task could be claimed before Vivaldi was ready. If browser setup then failed, destructive cleanup could reset Vivaldi and force the next task through first-run screens again.

Corrected lifecycle:
1. verify emulator readiness
2. make Vivaldi ready
3. handle notification permission and onboarding
4. claim a task
5. execute with bounded retries
6. persist the task result
7. clean up only after a genuine claimed-task execution path

## Result

The R5 baseline passed 56 automated tests, built cleanly with warnings-as-errors, published as a self-contained Windows x64 single-file application, and passed a packaged EXE startup smoke test.

Live four-instance acceptance remains a target-environment check and is not conflated with automated regression evidence.
