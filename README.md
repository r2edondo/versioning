# on master
./g printVersion
    if no tag 0.0.0 -snapshot
./g release
    null -> infer version -> 0.0.0-snapshot -> release -> 0.0.0
    repo tag 0.0.0
./g printVersion
    0.0.1-SNAPSHOT
./g printVersion cope=MAJOR
    1.0.0-SNAPSHOT

??? allowedReleasebranch = master
??? versionScope/releaseScope = patch

./g release
    0.0.0 -> infer version(scope, branch, clean?) -> 0.0.1-snapshot -> release -> 0.0.1
./g release -scope=minor
    0.0.1 -> infer version(scope, branch, clean?) -> 0.1.0-snapshot -> release -> 0.1.0
./g publish
    0.1.0 -> infer version(scope, branch, clean?) -> 0.1.1-snapshot -> publish

# on branch /feature/IG-123/a-b-c

./g printVersion
    0.1.0 -> infer version(scope, branch, clean?) -> 0.1.1+ig123
nops ??? this could add a hidden file .version to automate scope selection
buuuut ??? it could infer that from the branch name bugfix, feature
./g printVersion scope=major
    0.1.0 -> infer version(scope, branch, clean?) -> 1.0.0+ig123

# pipeline
push branch
    0.1.0 -> infer version(scope, branch, clean?) -> 0.1.1+ig123 -> publish -> deploy -> test
    makes no sense to ask for the versionScope, infer from the branch name

merge to master
    0.1.0 -> ?scope=minor infer-> 0.2.0-SNAPSHOT -> promote-release -> 0.2.0


code / compile / test / build / publish / i test / promote QA / i test / promote LIVE / i test
commit a...            x-snapshot                 x-final                 x-final



