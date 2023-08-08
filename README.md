# gradle-resolve-mutations-bug

Reproduces https://discuss.gradle.org/t/getbytype-or-withtype-failing-in-composite-build/42136

## Instructions

### All subprojects disabled

fails

```
$ ./gradlew build publishToMavenLocal integrationTest

./gradlew build publishToMavenLocal integrationTest                                                                                                                           
Unable to make progress running work. The following items are queued for execution but none of them can be started:
  - Build ':':
      - Waiting for nodes:
          - :disabled:publishToMavenLocal (state=SHOULD_RUN, dependencies=NOT_COMPLETE, group=task group 1, dependencies=[:disabled:publishExtraPackagePublicationToMavenLocal (SHOULD_RUN), :disabled:publishFullPackagePublicationToMavenLocal (EXECUTED)], waiting-for=[:disabled:publishExtraPackagePublicationToMavenLocal (SHOULD_RUN)], has-failed-dependency=false )
          - :enabled:publishToMavenLocal (state=SHOULD_RUN, dependencies=NOT_COMPLETE, group=task group 1, dependencies=[:enabled:publishExtraPackagePublicationToMavenLocal (SHOULD_RUN), :enabled:publishFullPackagePublicationToMavenLocal (EXECUTED)], waiting-for=[:enabled:publishExtraPackagePublicationToMavenLocal (SHOULD_RUN)], has-failed-dependency=false )
          - :disabled:integrationTest (state=SHOULD_RUN, dependencies=NOT_COMPLETE, group=task group 2, dependencies=[destroyer locations for task group 1 (SHOULD_RUN), :disabled:compileIntegrationTestJava (SHOULD_RUN), :disabled:integrationTestClasses (SHOULD_RUN)], waiting-for=[:disabled:compileIntegrationTestJava (SHOULD_RUN), destroyer locations for task group 1 (SHOULD_RUN), :disabled:integrationTestClasses (SHOULD_RUN)], has-failed-dependency=false )
          - :enabled:integrationTest (state=SHOULD_RUN, dependencies=NOT_COMPLETE, group=task group 2, dependencies=[destroyer locations for task group 1 (SHOULD_RUN), :enabled:compileIntegrationTestJava (SHOULD_RUN), :enabled:integrationTestClasses (SHOULD_RUN)], waiting-for=[:enabled:integrationTestClasses (SHOULD_RUN), destroyer locations for task group 1 (SHOULD_RUN), :enabled:compileIntegrationTestJava (SHOULD_RUN)], has-failed-dependency=false )
          - :buildDashboard (state=SHOULD_RUN, dependencies=NOT_COMPLETE, group=finalizer :buildDashboard ordinal: task group 0, delegate: default group, no dependencies, finalizes=[:disabled:integrationTest (SHOULD_RUN), :disabled:test (EXECUTED), :enabled:integrationTest (SHOULD_RUN), :enabled:test (EXECUTED)] )
      - Reachable nodes:
          - Resolve mutations for :buildDashboard (state=SHOULD_RUN, dependencies=NOT_COMPLETE, group=default group, no dependencies )
          - producer locations for task group 0 (state=SHOULD_RUN, dependencies=NOT_COMPLETE, group=task group 0, dependencies=[Resolve mutations for :buildDashboard (SHOULD_RUN), Resolve mutations for :enabled:test (EXECUTED), Resolve mutations for :enabled:processTestResources (EXECUTED), Resolve mutations for :enabled:compileTestJava (EXECUTED), Resolve mutations for :enabled:jar (EXECUTED), Resolve mutations for :enabled:processResources (EXECUTED), Resolve mutations for :enabled:compileJava (EXECUTED), Resolve mutations for :disabled:test (EXECUTED), Resolve mutations for :disabled:processTestResources (EXECUTED), Resolve mutations for :disabled:compileTestJava (EXECUTED), Resolve mutations for :disabled:jar (EXECUTED), Resolve mutations for :disabled:processResources (EXECUTED), Resolve mutations for :disabled:compileJava (EXECUTED)], waiting-for=[Resolve mutations for :buildDashboard (SHOULD_RUN)], has-failed-dependency=false )
          - :disabled:cleanExtra_disabled (state=SHOULD_RUN, dependencies=NOT_COMPLETE, group=task group 1, dependencies=[producer locations for task group 0 (SHOULD_RUN), :disabled:check (EXECUTED)], waiting-for=[producer locations for task group 0 (SHOULD_RUN)], has-failed-dependency=false )
          - :disabled:extraZip (state=SHOULD_RUN, dependencies=NOT_COMPLETE, group=task group 1, dependencies=[destroyer locations for task group 0 (EXECUTED), :disabled:cleanExtra_disabled (SHOULD_RUN)], waiting-for=[:disabled:cleanExtra_disabled (SHOULD_RUN)], has-failed-dependency=false )
          - :disabled:publishExtraPackagePublicationToMavenLocal (state=SHOULD_RUN, dependencies=NOT_COMPLETE, group=task group 1, dependencies=[:disabled:extraZip (SHOULD_RUN), :disabled:generatePomFileForExtraPackagePublication (EXECUTED)], waiting-for=[:disabled:extraZip (SHOULD_RUN)], has-failed-dependency=false )
          - :enabled:cleanExtra_enabled (state=SHOULD_RUN, dependencies=NOT_COMPLETE, group=task group 1, dependencies=[producer locations for task group 0 (SHOULD_RUN), :enabled:check (EXECUTED)], waiting-for=[producer locations for task group 0 (SHOULD_RUN)], has-failed-dependency=false )
          - :enabled:extraZip (state=SHOULD_RUN, dependencies=NOT_COMPLETE, group=task group 1, dependencies=[destroyer locations for task group 0 (EXECUTED), :enabled:cleanExtra_enabled (SHOULD_RUN)], waiting-for=[:enabled:cleanExtra_enabled (SHOULD_RUN)], has-failed-dependency=false )
          - :enabled:publishExtraPackagePublicationToMavenLocal (state=SHOULD_RUN, dependencies=NOT_COMPLETE, group=task group 1, dependencies=[:enabled:extraZip (SHOULD_RUN), :enabled:generatePomFileForExtraPackagePublication (EXECUTED)], waiting-for=[:enabled:extraZip (SHOULD_RUN)], has-failed-dependency=false )
          - Resolve mutations for :enabled:cleanExtra_enabled (state=SHOULD_RUN, dependencies=NOT_COMPLETE, group=default group, no dependencies )
          - Resolve mutations for :disabled:cleanExtra_disabled (state=SHOULD_RUN, dependencies=NOT_COMPLETE, group=default group, no dependencies )
          - destroyer locations for task group 1 (state=SHOULD_RUN, dependencies=NOT_COMPLETE, group=task group 1, dependencies=[Resolve mutations for :enabled:cleanExtra_enabled (SHOULD_RUN), Resolve mutations for :disabled:cleanExtra_disabled (SHOULD_RUN), destroyer locations for task group 0 (EXECUTED)], waiting-for=[Resolve mutations for :enabled:cleanExtra_enabled (SHOULD_RUN), Resolve mutations for :disabled:cleanExtra_disabled (SHOULD_RUN)], has-failed-dependency=false )
          - :disabled:compileIntegrationTestJava (state=SHOULD_RUN, dependencies=NOT_COMPLETE, group=task group 2, dependencies=[destroyer locations for task group 1 (SHOULD_RUN)], waiting-for=[destroyer locations for task group 1 (SHOULD_RUN)], has-failed-dependency=false )
          - :disabled:processIntegrationTestResources (state=SHOULD_RUN, dependencies=NOT_COMPLETE, group=task group 2, dependencies=[destroyer locations for task group 1 (SHOULD_RUN)], waiting-for=[destroyer locations for task group 1 (SHOULD_RUN)], has-failed-dependency=false )
          - :disabled:integrationTestClasses (state=SHOULD_RUN, dependencies=NOT_COMPLETE, group=task group 2, dependencies=[:disabled:compileIntegrationTestJava (SHOULD_RUN), :disabled:processIntegrationTestResources (SHOULD_RUN)], waiting-for=[:disabled:compileIntegrationTestJava (SHOULD_RUN), :disabled:processIntegrationTestResources (SHOULD_RUN)], has-failed-dependency=false )
          - :enabled:compileIntegrationTestJava (state=SHOULD_RUN, dependencies=NOT_COMPLETE, group=task group 2, dependencies=[destroyer locations for task group 1 (SHOULD_RUN)], waiting-for=[destroyer locations for task group 1 (SHOULD_RUN)], has-failed-dependency=false )
          - :enabled:processIntegrationTestResources (state=SHOULD_RUN, dependencies=NOT_COMPLETE, group=task group 2, dependencies=[destroyer locations for task group 1 (SHOULD_RUN)], waiting-for=[destroyer locations for task group 1 (SHOULD_RUN)], has-failed-dependency=false )
          - :enabled:integrationTestClasses (state=SHOULD_RUN, dependencies=NOT_COMPLETE, group=task group 2, dependencies=[:enabled:compileIntegrationTestJava (SHOULD_RUN), :enabled:processIntegrationTestResources (SHOULD_RUN)], waiting-for=[:enabled:processIntegrationTestResources (SHOULD_RUN), :enabled:compileIntegrationTestJava (SHOULD_RUN)], has-failed-dependency=false )
      - Scheduling events:
          - node added to plan: producer locations for task group 0, when: scheduled, state: SHOULD_RUN, dependencies: 13, is ready node? false
          - node added to plan: destroyer locations for task group 0, when: scheduled, state: SHOULD_RUN, dependencies: 0, is ready node? true
          - node added to plan: destroyer locations for task group 1, when: scheduled, state: SHOULD_RUN, dependencies: 3, is ready node? false
      - Ordinal groups:
          - group 0 entry nodes: [:build (EXECUTED), :disabled:build (EXECUTED), :enabled:build (EXECUTED)]
          - group 1 entry nodes: [:disabled:publishToMavenLocal (SHOULD_RUN), :enabled:publishToMavenLocal (SHOULD_RUN)]
          - group 2 entry nodes: [:disabled:integrationTest (SHOULD_RUN), :enabled:integrationTest (SHOULD_RUN)]
  - Workers waiting for work: 12
  - Stopped workers: 0

FAILURE: Build failed with an exception.
```

### At least one subproject disabled

fails

NB: doesn't matter which ones 

```
 ./gradlew build publishToMavenLocal integrationTest -Pdisabled.zipEnable=t                                                                                                    

> Configure project :disabled
disabled.zipEnable is set
Unable to make progress running work. The following items are queued for execution but none of them can be started:
  - Build ':':
      - Waiting for nodes:
          - :disabled:publishToMavenLocal (state=SHOULD_RUN, dependencies=NOT_COMPLETE, group=task group 1, dependencies=[:disabled:publishExtraPackagePublicationToMavenLocal (SHOULD_RUN), :disabled:publishFullPackagePublicationToMavenLocal (EXECUTED)], waiting-for=[:disabled:publishExtraPackagePublicationToMavenLocal (SHOULD_RUN)], has-failed-dependency=false )
          - :enabled:publishToMavenLocal (state=SHOULD_RUN, dependencies=NOT_COMPLETE, group=task group 1, dependencies=[:enabled:publishExtraPackagePublicationToMavenLocal (SHOULD_RUN), :enabled:publishFullPackagePublicationToMavenLocal (EXECUTED)], waiting-for=[:enabled:publishExtraPackagePublicationToMavenLocal (SHOULD_RUN)], has-failed-dependency=false )
          - :buildDashboard (state=SHOULD_RUN, dependencies=NOT_COMPLETE, group=finalizer :buildDashboard ordinal: task group 0, delegate: default group, no dependencies, finalizes=[:disabled:integrationTest (EXECUTED), :disabled:test (EXECUTED), :enabled:integrationTest (SHOULD_RUN), :enabled:test (EXECUTED)] )
          - :enabled:integrationTest (state=SHOULD_RUN, dependencies=NOT_COMPLETE, group=task group 2, dependencies=[destroyer locations for task group 1 (SHOULD_RUN), :enabled:compileIntegrationTestJava (SHOULD_RUN), :enabled:integrationTestClasses (SHOULD_RUN)], waiting-for=[destroyer locations for task group 1 (SHOULD_RUN), :enabled:compileIntegrationTestJava (SHOULD_RUN), :enabled:integrationTestClasses (SHOULD_RUN)], has-failed-dependency=false )
      - Reachable nodes:
          - Resolve mutations for :buildDashboard (state=SHOULD_RUN, dependencies=NOT_COMPLETE, group=default group, no dependencies )
          - producer locations for task group 0 (state=SHOULD_RUN, dependencies=NOT_COMPLETE, group=task group 0, dependencies=[Resolve mutations for :buildDashboard (SHOULD_RUN), Resolve mutations for :enabled:test (EXECUTED), Resolve mutations for :enabled:processTestResources (EXECUTED), Resolve mutations for :enabled:compileTestJava (EXECUTED), Resolve mutations for :enabled:jar (EXECUTED), Resolve mutations for :enabled:processResources (EXECUTED), Resolve mutations for :enabled:compileJava (EXECUTED), Resolve mutations for :disabled:test (EXECUTED), Resolve mutations for :disabled:processTestResources (EXECUTED), Resolve mutations for :disabled:compileTestJava (EXECUTED), Resolve mutations for :disabled:jar (EXECUTED), Resolve mutations for :disabled:processResources (EXECUTED), Resolve mutations for :disabled:compileJava (EXECUTED)], waiting-for=[Resolve mutations for :buildDashboard (SHOULD_RUN)], has-failed-dependency=false )
          - :disabled:cleanExtra_disabled (state=SHOULD_RUN, dependencies=NOT_COMPLETE, group=task group 1, dependencies=[producer locations for task group 0 (SHOULD_RUN), :disabled:check (EXECUTED)], waiting-for=[producer locations for task group 0 (SHOULD_RUN)], has-failed-dependency=false )
          - :disabled:extraZip (state=SHOULD_RUN, dependencies=NOT_COMPLETE, group=task group 1, dependencies=[destroyer locations for task group 0 (EXECUTED), :disabled:cleanExtra_disabled (SHOULD_RUN)], waiting-for=[:disabled:cleanExtra_disabled (SHOULD_RUN)], has-failed-dependency=false )
          - :disabled:publishExtraPackagePublicationToMavenLocal (state=SHOULD_RUN, dependencies=NOT_COMPLETE, group=task group 1, dependencies=[:disabled:extraZip (SHOULD_RUN), :disabled:generatePomFileForExtraPackagePublication (EXECUTED)], waiting-for=[:disabled:extraZip (SHOULD_RUN)], has-failed-dependency=false )
          - :enabled:cleanExtra_enabled (state=SHOULD_RUN, dependencies=NOT_COMPLETE, group=task group 1, dependencies=[producer locations for task group 0 (SHOULD_RUN), :enabled:check (EXECUTED)], waiting-for=[producer locations for task group 0 (SHOULD_RUN)], has-failed-dependency=false )
          - :enabled:extraZip (state=SHOULD_RUN, dependencies=NOT_COMPLETE, group=task group 1, dependencies=[destroyer locations for task group 0 (EXECUTED), :enabled:cleanExtra_enabled (SHOULD_RUN)], waiting-for=[:enabled:cleanExtra_enabled (SHOULD_RUN)], has-failed-dependency=false )
          - :enabled:publishExtraPackagePublicationToMavenLocal (state=SHOULD_RUN, dependencies=NOT_COMPLETE, group=task group 1, dependencies=[:enabled:extraZip (SHOULD_RUN), :enabled:generatePomFileForExtraPackagePublication (EXECUTED)], waiting-for=[:enabled:extraZip (SHOULD_RUN)], has-failed-dependency=false )
          - Resolve mutations for :enabled:cleanExtra_enabled (state=SHOULD_RUN, dependencies=NOT_COMPLETE, group=default group, no dependencies )
          - Resolve mutations for :disabled:cleanExtra_disabled (state=SHOULD_RUN, dependencies=NOT_COMPLETE, group=default group, no dependencies )
          - destroyer locations for task group 1 (state=SHOULD_RUN, dependencies=NOT_COMPLETE, group=task group 1, dependencies=[Resolve mutations for :enabled:cleanExtra_enabled (SHOULD_RUN), Resolve mutations for :disabled:cleanExtra_disabled (SHOULD_RUN), destroyer locations for task group 0 (EXECUTED)], waiting-for=[Resolve mutations for :enabled:cleanExtra_enabled (SHOULD_RUN), Resolve mutations for :disabled:cleanExtra_disabled (SHOULD_RUN)], has-failed-dependency=false )
          - :enabled:compileIntegrationTestJava (state=SHOULD_RUN, dependencies=NOT_COMPLETE, group=task group 2, dependencies=[destroyer locations for task group 1 (SHOULD_RUN)], waiting-for=[destroyer locations for task group 1 (SHOULD_RUN)], has-failed-dependency=false )
          - :enabled:processIntegrationTestResources (state=SHOULD_RUN, dependencies=NOT_COMPLETE, group=task group 2, dependencies=[destroyer locations for task group 1 (SHOULD_RUN)], waiting-for=[destroyer locations for task group 1 (SHOULD_RUN)], has-failed-dependency=false )
          - :enabled:integrationTestClasses (state=SHOULD_RUN, dependencies=NOT_COMPLETE, group=task group 2, dependencies=[:enabled:compileIntegrationTestJava (SHOULD_RUN), :enabled:processIntegrationTestResources (SHOULD_RUN)], waiting-for=[:enabled:processIntegrationTestResources (SHOULD_RUN), :enabled:compileIntegrationTestJava (SHOULD_RUN)], has-failed-dependency=false )
      - Scheduling events:
          - node added to plan: producer locations for task group 0, when: scheduled, state: SHOULD_RUN, dependencies: 13, is ready node? false
          - node added to plan: destroyer locations for task group 0, when: scheduled, state: SHOULD_RUN, dependencies: 0, is ready node? true
          - node added to plan: destroyer locations for task group 1, when: scheduled, state: SHOULD_RUN, dependencies: 3, is ready node? false
      - Ordinal groups:
          - group 0 entry nodes: [:build (EXECUTED), :disabled:build (EXECUTED), :enabled:build (EXECUTED)]
          - group 1 entry nodes: [:disabled:publishToMavenLocal (SHOULD_RUN), :enabled:publishToMavenLocal (SHOULD_RUN)]
          - group 2 entry nodes: [:disabled:integrationTest (EXECUTED), :enabled:integrationTest (SHOULD_RUN)]
  - Workers waiting for work: 12
  - Stopped workers: 0

FAILURE: Build failed with an exception.

* What went wrong:
Unable to make progress running work. There are items queued for execution but none of them can be started

```

### All enabled

succeeds

```
./gradlew build publishToMavenLocal integrationTest -Pdisabled.zipEnable=t -Penabled.zipEnable=t                                                                       

> Configure project :disabled
disabled.zipEnable is set

> Configure project :enabled
enabled.zipEnable is set

Deprecated Gradle features were used in this build, making it incompatible with Gradle 9.0.

You can use '--warning-mode all' to show the individual deprecation warnings and determine if they come from your own scripts or plugins.

For more on this, please refer to https://docs.gradle.org/8.2.1/userguide/command_line_interface.html#sec:command_line_warnings in the Gradle documentation.

BUILD SUCCESSFUL in 1s
29 actionable tasks: 16 executed, 13 up-to-date
```
