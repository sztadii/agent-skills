---
name: proof
description: Trigger this if the user says 'proof', or another skill invokes it.
disable-model-invocation: true
---

# Proof

Verify actual changed behavior locally. Authorizes ordinary setup and execution,
not publication, production changes, or destructive actions.

```ts
async function proof(scope) {
  const running = await confirmRevisionEnvironmentAndData(scope);
  if (!running.hasRunnableBehavior) return validateContentAndLinks(scope);

  if (!await liveDependencyAvailable()) {
    await trySimulatorFixtureOrMinimalDependencyMock(); // Exercise actual UI; label mocks.
  }
  await reproduceOnBaseOrShowPriorExperience(scope);
  await exerciseChangedFlowAndFailureCases(scope); // Actual UI, endpoint or CLI.
  await runRepositoryRequiredChecks(scope);

  if (scope.visual) {
    const captures = await captureComparableBeforeAndAfter({
      format: scope.interaction ? "recording" : "screenshots",
    });
    await openCapturesAndPlayRecordings(captures);
  } else {
    await retainCommandsAndObservedOutput();
  }
  return { snapshot: running.snapshot, ...await collectChecksArtifactsAndGaps() };
}
```

Identify evidence by revision and local diff, including untracked task files.
Match inputs, app state, viewport and framing across captures. For permission
changes, exercise the action with and without permission. An offline device is
not by itself a recording blocker. Tests, source inspection and HTTP 200 do not
replace exercising the behavior or recording the changed interaction.

Use an isolated base without overwriting work. Report missing regression
coverage to the implementation workflow; read-only reviews do not edit author code.
After behavior edits or conflict fixes, rerun affected verification. Mock success
does not prove the live integration. If blocked, retain attempts and name exactly
what remains unverified; never claim complete proof. Keep artifacts local unless
the calling workflow authorizes publication; pass inspected evidence to file-pr.
