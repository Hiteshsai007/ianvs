# Exhaustive link audit — current Example Restoration Discussions

**Audit date:** 2026-08-25 (Asia/Calcutta)  
**Scope:** the 41 current Show-and-tell Discussions whose titles contain `Example Restoration` (including #864 for census purposes; its content is excluded from substantive scoring).

## Coverage

- Discussions fetched: **41/41**
- Discussion comments fetched and inspected: **128**
- Unique canonicalized URLs extracted from bodies/comments: **323**
- Every extracted URL is listed below with its source Discussion(s) and observed resolution state.

The GitHub repository, issue, pull-request, commit, blob, user, and Discussion URLs were routed through the corresponding GitHub API endpoint where identifiable. The 37 `#discussioncomment-...` links initially returned 404 only because the attempted generic REST route was wrong; each ID was independently matched to a comment record in the fetched Discussion snapshot, so they are reported as resolved rather than broken. A retry without redirect-following showed the 52 GitHub user-attachment routes returning HTTP 302; the sandbox could not complete their binary fetch because the redirected asset transport closed with TLS EOF. The three raw-image links were independently verified through the GitHub Contents API at their pinned refs/paths. The YouTube URL also hit a TLS EOF and could not be playback-verified here. These network limitations are recorded rather than silently treated as successful content retrieval.

## Aggregate observed states

| Observed state | Count | Interpretation |
|---|---:|---|
| HTTP 200 | 230 | 222 API responses plus 8 web responses |
| Discussion comment resolved from fetched record | 37 | The attempted generic `/discussions/comments/{id}` route was wrong; the comment ID exists in the fetched current Discussion data |
| Attachment route HTTP 302, binary fetch TLS-failed | 52 | Route is live at the redirect layer; asset bytes were not independently retrieved in this sandbox |
| Raw asset verified through GitHub Contents API | 3 | Pinned blob/path exists despite raw-domain TLS failure |
| YouTube TLS transport failure | 1 | URL remains linked evidence; playback is not independently verified |

The 37 comment-route 404s are not counted as broken links. No canonical GitHub issue/PR/commit/blob/user URL returned 404 in the corrected API pass. Bare `#` placeholders and malformed/non-URL text are not included in the 323-URL count and are called out in the ranking where they affect evidence quality.

## Per-URL record

| Source Discussion(s) | Mentioned URL | Observed state | Detail |
|---|---|---|---|

| #856 | <https://github.com/user-attachments/assets/1e59fc95-a3aa-47f5-a23e-f01515c6f938> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #856 | <https://github.com/user-attachments/assets/95f0c240-6943-4881-bc72-6cd2ff532dc7> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #856 | <https://github.com/user-attachments/assets/ab545624-e730-4a1d-acb7-b5b737fe245a> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #862 | <https://github.com/kubeedge/ianvs/pull/854> | HTTP 200 | api route |
| #864 | <https://github.com/kubeedge/ianvs/discussions/864#discussioncomment-18118430> | resolved against fetched Discussion comment | snapshot/API comment record |
| #864 | <https://github.com/kubeedge/ianvs/discussions/864#discussioncomment-18118431> | resolved against fetched Discussion comment | snapshot/API comment record |
| #864 | <https://github.com/kubeedge/ianvs/discussions/864#discussioncomment-18118433> | resolved against fetched Discussion comment | snapshot/API comment record |
| #864 | <https://github.com/kubeedge/ianvs/discussions/864#discussioncomment-18118434> | resolved against fetched Discussion comment | snapshot/API comment record |
| #864 | <https://github.com/kubeedge/ianvs/discussions/864#discussioncomment-18118436> | resolved against fetched Discussion comment | snapshot/API comment record |
| #864 | <https://github.com/kubeedge/ianvs/issues/604#issuecomment-5381811511> | HTTP 200 | api route |
| #864 | <https://github.com/kubeedge/ianvs/issues/742#issuecomment-5381812036> | HTTP 200 | api route |
| #864 | <https://github.com/kubeedge/ianvs/issues/823#issuecomment-5381811769> | HTTP 200 | api route |
| #864 | <https://github.com/kubeedge/ianvs/pull/661#issuecomment-5381815090> | HTTP 200 | api route |
| #864 | <https://github.com/kubeedge/ianvs/pull/682#issuecomment-5381815655> | HTTP 200 | api route |
| #864 | <https://github.com/kubeedge/ianvs/pull/744#issuecomment-5381815928> | HTTP 200 | api route |
| #864 | <https://github.com/kubeedge/ianvs/pull/758#issuecomment-5381813434> | HTTP 200 | api route |
| #864 | <https://github.com/kubeedge/ianvs/pull/789#issuecomment-5381813715> | HTTP 200 | api route |
| #864 | <https://github.com/kubeedge/ianvs/pull/791#issuecomment-5381815365> | HTTP 200 | api route |
| #864 | <https://github.com/kubeedge/ianvs/pull/796#issuecomment-5381813133> | HTTP 200 | api route |
| #864 | <https://github.com/kubeedge/ianvs/pull/824#issuecomment-5381812859> | HTTP 200 | api route |
| #864 | <https://github.com/kubeedge/ianvs/pull/835#issuecomment-5381816174> | HTTP 200 | api route |
| #864 | <https://github.com/kubeedge/ianvs/pull/849#issuecomment-5381813987> | HTTP 200 | api route |
| #865 | <https://github.com/prachi194agrawal> | HTTP 200 | api route |
| #866 | <https://github.com/kubeedge/ianvs/discussions/866#discussioncomment-18119107> | resolved against fetched Discussion comment | snapshot/API comment record |
| #866 | <https://github.com/kubeedge/ianvs/discussions/866#discussioncomment-18119108> | resolved against fetched Discussion comment | snapshot/API comment record |
| #866 | <https://github.com/kubeedge/ianvs/discussions/866#discussioncomment-18119109> | resolved against fetched Discussion comment | snapshot/API comment record |
| #866 | <https://github.com/kubeedge/ianvs/discussions/866#discussioncomment-18119110> | resolved against fetched Discussion comment | snapshot/API comment record |
| #866 | <https://github.com/kubeedge/ianvs/discussions/866#discussioncomment-18119111> | resolved against fetched Discussion comment | snapshot/API comment record |
| #867 | <https://github.com/hariomphulre> | HTTP 200 | api route |
| #868 | <https://github.com/kubeedge/ianvs/discussions/868> | HTTP 200 | api route |
| #869 | <https://github.com/kubeedge/ianvs/issues/816> | HTTP 200 | api route |
| #869 | <https://github.com/kubeedge/ianvs/issues/816#issuecomment-5385629623> | HTTP 200 | api route |
| #869 | <https://github.com/kubeedge/ianvs/issues/816#issuecomment-5385664021> | HTTP 200 | api route |
| #869 | <https://github.com/kubeedge/ianvs/issues/816#issuecomment-5385685951> | HTTP 200 | api route |
| #869 | <https://github.com/kubeedge/ianvs/issues/840> | HTTP 200 | api route |
| #869 | <https://github.com/kubeedge/ianvs/issues/840#issuecomment-5385628145> | HTTP 200 | api route |
| #869 | <https://github.com/kubeedge/ianvs/issues/840#issuecomment-5385663260> | HTTP 200 | api route |
| #869 | <https://github.com/kubeedge/ianvs/issues/840#issuecomment-5385682907> | HTTP 200 | api route |
| #869, #921 | <https://github.com/kubeedge/ianvs/issues/842> | HTTP 200 | api route |
| #869 | <https://github.com/kubeedge/ianvs/issues/842#issuecomment-5385662096> | HTTP 200 | api route |
| #869 | <https://github.com/kubeedge/ianvs/issues/842#issuecomment-5385678502> | HTTP 200 | api route |
| #869 | <https://github.com/kubeedge/ianvs/issues/842#issuecomment-5385686360> | HTTP 200 | api route |
| #869 | <https://github.com/kubeedge/ianvs/issues/842#issuecomment-5385706220> | HTTP 200 | api route |
| #869 | <https://github.com/kubeedge/ianvs/issues/842#issuecomment-5385716303> | HTTP 200 | api route |
| #869 | <https://github.com/kubeedge/ianvs/issues/842#issuecomment-5385720845> | HTTP 200 | api route |
| #869 | <https://github.com/kubeedge/ianvs/pull/676#issuecomment-5386900668> | HTTP 200 | api route |
| #869 | <https://github.com/kubeedge/ianvs/pull/688#issuecomment-5386902744> | HTTP 200 | api route |
| #869, #921 | <https://github.com/kubeedge/ianvs/pull/803> | HTTP 200 | api route |
| #869 | <https://github.com/kubeedge/ianvs/pull/803#issuecomment-5385706734> | HTTP 200 | api route |
| #869 | <https://github.com/kubeedge/ianvs/pull/817> | HTTP 200 | api route |
| #869 | <https://github.com/kubeedge/ianvs/pull/817#issuecomment-5385710379> | HTTP 200 | api route |
| #869 | <https://github.com/kubeedge/ianvs/pull/817#issuecomment-5400173605> | HTTP 200 | api route |
| #869, #921 | <https://github.com/kubeedge/ianvs/pull/843> | HTTP 200 | api route |
| #869 | <https://github.com/kubeedge/ianvs/pull/843#issuecomment-5385702594> | HTTP 200 | api route |
| #869 | <https://github.com/kubeedge/ianvs/pull/843#issuecomment-5387001912> | HTTP 200 | api route |
| #869 | <https://github.com/kubeedge/ianvs/pull/843#issuecomment-5387608414> | HTTP 200 | api route |
| #869 | <https://github.com/kubeedge/ianvs/pull/843#issuecomment-5387621277> | HTTP 200 | api route |
| #870, #882, #900 | <https://github.com/kubeedge/ianvs.git> | HTTP 200 | web route |
| #870 | <https://github.com/kubeedge/ianvs/discussions/870#discussioncomment-18120175> | resolved against fetched Discussion comment | snapshot/API comment record |
| #870 | <https://github.com/kubeedge/ianvs/discussions/870#discussioncomment-18120317> | resolved against fetched Discussion comment | snapshot/API comment record |
| #870 | <https://github.com/kubeedge/ianvs/discussions/870#discussioncomment-18120335> | resolved against fetched Discussion comment | snapshot/API comment record |
| #870 | <https://github.com/kubeedge/ianvs/issues/489#issuecomment-5383178292> | HTTP 200 | api route |
| #870 | <https://github.com/kubeedge/ianvs/issues/489#issuecomment-5385582803> | HTTP 200 | api route |
| #870 | <https://github.com/kubeedge/ianvs/issues/840#issuecomment-5383175561> | HTTP 200 | api route |
| #870 | <https://github.com/kubeedge/ianvs/issues/840#issuecomment-5385578739> | HTTP 200 | api route |
| #870 | <https://github.com/kubeedge/ianvs/pull/510#issuecomment-5383249921> | HTTP 200 | api route |
| #870 | <https://github.com/kubeedge/ianvs/pull/682#issuecomment-5383252526> | HTTP 200 | api route |
| #870 | <https://github.com/kubeedge/ianvs/pull/841#issuecomment-5383247526> | HTTP 200 | api route |
| #870 | <https://github.com/kubeedge/sedna.git> | HTTP 200 | web route |
| #873 | <https://github.com/kubeedge/ianvs/discussions/873#discussioncomment-18121028> | resolved against fetched Discussion comment | snapshot/API comment record |
| #873 | <https://github.com/kubeedge/ianvs/discussions/873#discussioncomment-18121848> | resolved against fetched Discussion comment | snapshot/API comment record |
| #873 | <https://github.com/kubeedge/ianvs/discussions/873#discussioncomment-18121964> | resolved against fetched Discussion comment | snapshot/API comment record |
| #873 | <https://github.com/kubeedge/ianvs/discussions/873#discussioncomment-18121990> | resolved against fetched Discussion comment | snapshot/API comment record |
| #873 | <https://github.com/kubeedge/ianvs/discussions/873#discussioncomment-18128906> | resolved against fetched Discussion comment | snapshot/API comment record |
| #873 | <https://github.com/kubeedge/ianvs/issues/462#issuecomment-5389810199> | HTTP 200 | api route |
| #873 | <https://github.com/kubeedge/ianvs/issues/591#issuecomment-5389827111> | HTTP 200 | api route |
| #873 | <https://github.com/kubeedge/ianvs/issues/740#issuecomment-5389838120> | HTTP 200 | api route |
| #873 | <https://github.com/kubeedge/ianvs/pull/251#issuecomment-5390154427> | HTTP 200 | api route |
| #873 | <https://github.com/kubeedge/ianvs/pull/384#issuecomment-5389920060> | HTTP 200 | api route |
| #873 | <https://github.com/kubeedge/ianvs/pull/496#issuecomment-5389994245> | HTTP 200 | api route |
| #873 | <https://github.com/kubeedge/ianvs/pull/588#issuecomment-5389967393> | HTTP 200 | api route |
| #873 | <https://github.com/kubeedge/ianvs/pull/592#issuecomment-5389938597> | HTTP 200 | api route |
| #873 | <https://github.com/kubeedge/ianvs/pull/638#issuecomment-5397652904> | HTTP 200 | api route |
| #873 | <https://github.com/kubeedge/ianvs/pull/648#issuecomment-5390182391> | HTTP 200 | api route |
| #873 | <https://github.com/kubeedge/ianvs/pull/657#issuecomment-5397421926> | HTTP 200 | api route |
| #873 | <https://github.com/kubeedge/ianvs/pull/741#issuecomment-5389954427> | HTTP 200 | api route |
| #873 | <https://github.com/kubeedge/ianvs/pull/755#issuecomment-5390170221> | HTTP 200 | api route |
| #873 | <https://github.com/kubeedge/ianvs/pull/817#issuecomment-5390130595> | HTTP 200 | api route |
| #873 | <https://github.com/kubeedge/ianvs/pull/825#issuecomment-5397697227> | HTTP 200 | api route |
| #875 | <https://github.com/Hiteshsai007> | HTTP 200 | api route |
| #875 | <https://github.com/kubeedge/ianvs/blob/28cff7eb87b8e9a4b1d3f6544716f395e5b465bd/core/testenvmanager/dataset/utils.py#L83-L91> | HTTP 200 | api route |
| #875 | <https://github.com/kubeedge/ianvs/blob/2d8343424dca334b88621ddffc7e811b818fd757/examples/MOT17/multiedge_inference_bench/pedestrian_tracking/testalgorithms/reid/m3l/basemodel.py#L189> | HTTP 200 | api route |
| #875 | <https://github.com/kubeedge/ianvs/blob/2d8343424dca334b88621ddffc7e811b818fd757/examples/MOT17/multiedge_inference_bench/pedestrian_tracking/testalgorithms/reid/m3l/basemodel.py#L43-L49> | HTTP 200 | api route |
| #875 | <https://github.com/kubeedge/ianvs/blob/2d8343424dca334b88621ddffc7e811b818fd757/examples/MOT17/multiedge_inference_bench/pedestrian_tracking/testalgorithms/reid/m3l/basemodel.py#L67-L75> | HTTP 200 | api route |
| #875 | <https://github.com/kubeedge/ianvs/blob/2d8343424dca334b88621ddffc7e811b818fd757/examples/MOT17/multiedge_inference_bench/pedestrian_tracking/testalgorithms/reid/m3l/test_basemodel.py#L35-L41> | HTTP 200 | api route |
| #875 | <https://github.com/kubeedge/ianvs/blob/2d8343424dca334b88621ddffc7e811b818fd757/examples/MOT17/multiedge_inference_bench/pedestrian_tracking/testalgorithms/reid/m3l/test_basemodel.py#L44-L57> | HTTP 200 | api route |
| #875 | <https://github.com/kubeedge/ianvs/blob/2d8343424dca334b88621ddffc7e811b818fd757/examples/MOT17/multiedge_inference_bench/pedestrian_tracking/testalgorithms/reid/m3l/test_basemodel.py#L60-L83> | HTTP 200 | api route |
| #875 | <https://github.com/kubeedge/ianvs/blob/37a9c60a9747af0cfe3170f84249bc349c56e8d5/core/testcasecontroller/algorithm/paradigm/singletask_learning/singletask_learning.py#L113-L116> | HTTP 200 | api route |
| #875 | <https://github.com/kubeedge/ianvs/blob/6803326aed7876b47330a13949403d5ec3417f42/core/storymanager/rank/test_rank_sort.py#L19-L21> | HTTP 200 | api route |
| #875 | <https://github.com/kubeedge/ianvs/commit/28cff7eb87b8e9a4b1d3f6544716f395e5b465bd> | HTTP 200 | api route |
| #875 | <https://github.com/kubeedge/ianvs/commit/646b085829fa09d83a4f0c2730102aaaaf50cf9a> | HTTP 200 | api route |
| #875 | <https://github.com/kubeedge/ianvs/commit/6803326aed7876b47330a13949403d5ec3417f42> | HTTP 200 | api route |
| #875 | <https://github.com/kubeedge/ianvs/commit/77daad3a09868d57efab69d562bde38d0af3bd6b> | HTTP 200 | api route |
| #875 | <https://github.com/kubeedge/ianvs/commit/835f4457f387eff218541ca8d433b465dd960765> | HTTP 200 | api route |
| #875 | <https://github.com/kubeedge/ianvs/commit/89c2a013de1185d9ec137e1045c8337334aed861> | HTTP 200 | api route |
| #875 | <https://github.com/kubeedge/ianvs/commit/998290ebd9b23629a3ebb28a5c99b7a034768506> | HTTP 200 | api route |
| #875 | <https://github.com/kubeedge/ianvs/commit/beb23be64977b14f09a45fbf4cc8bfc32dc9633f> | HTTP 200 | api route |
| #875 | <https://github.com/kubeedge/ianvs/discussions/875#discussioncomment-18121621> | resolved against fetched Discussion comment | snapshot/API comment record |
| #875 | <https://github.com/kubeedge/ianvs/discussions/875#discussioncomment-18121643> | resolved against fetched Discussion comment | snapshot/API comment record |
| #875 | <https://github.com/kubeedge/ianvs/discussions/875#discussioncomment-18121677> | resolved against fetched Discussion comment | snapshot/API comment record |
| #875 | <https://github.com/kubeedge/ianvs/discussions/875#discussioncomment-18121695> | resolved against fetched Discussion comment | snapshot/API comment record |
| #875 | <https://github.com/kubeedge/ianvs/discussions/875#discussioncomment-18121701> | resolved against fetched Discussion comment | snapshot/API comment record |
| #875 | <https://github.com/kubeedge/ianvs/issues/523#issuecomment-5386498840> | HTTP 200 | api route |
| #875 | <https://github.com/kubeedge/ianvs/issues/535#issuecomment-5384537091> | HTTP 200 | api route |
| #875 | <https://github.com/kubeedge/ianvs/issues/765#issuecomment-5384535569> | HTTP 200 | api route |
| #875 | <https://github.com/kubeedge/ianvs/pull/524#issuecomment-5386501544> | HTTP 200 | api route |
| #875 | <https://github.com/kubeedge/ianvs/pull/536#issuecomment-5386505503> | HTTP 200 | api route |
| #875 | <https://github.com/kubeedge/ianvs/pull/701> | HTTP 200 | api route |
| #875 | <https://github.com/kubeedge/ianvs/pull/701#issuecomment-5409823176> | HTTP 200 | api route |
| #875 | <https://github.com/kubeedge/ianvs/pull/734> | HTTP 200 | api route |
| #875 | <https://github.com/kubeedge/ianvs/pull/734#issuecomment-5409817163> | HTTP 200 | api route |
| #875 | <https://github.com/kubeedge/ianvs/pull/764#issuecomment-5384563777> | HTTP 200 | api route |
| #875 | <https://github.com/kubeedge/ianvs/pull/767#issuecomment-5384561099> | HTTP 200 | api route |
| #875 | <https://github.com/kubeedge/ianvs/pull/773> | HTTP 200 | api route |
| #875 | <https://github.com/kubeedge/ianvs/pull/773#issuecomment-5409809791> | HTTP 200 | api route |
| #875 | <https://github.com/kubeedge/ianvs/pull/775> | HTTP 200 | api route |
| #875 | <https://github.com/kubeedge/ianvs/pull/775#issuecomment-5386643381> | HTTP 200 | api route |
| #875 | <https://github.com/kubeedge/ianvs/pull/802> | HTTP 200 | api route |
| #875 | <https://github.com/kubeedge/ianvs/pull/802#issuecomment-5386647117> | HTTP 200 | api route |
| #875 | <https://github.com/kubeedge/ianvs/pull/802#issuecomment-5387002670> | HTTP 200 | api route |
| #875 | <https://github.com/kubeedge/ianvs/pull/802#issuecomment-5387138574> | HTTP 200 | api route |
| #875 | <https://github.com/kubeedge/ianvs/pull/802#issuecomment-5387170752> | HTTP 200 | api route |
| #875 | <https://github.com/kubeedge/ianvs/pull/802#issuecomment-5387198216> | HTTP 200 | api route |
| #875 | <https://github.com/kubeedge/ianvs/pull/808> | HTTP 200 | api route |
| #875 | <https://github.com/kubeedge/ianvs/pull/808#issuecomment-5386552612> | HTTP 200 | api route |
| #875, #915 | <https://github.com/kubeedge/ianvs/pull/827> | HTTP 200 | api route |
| #875 | <https://github.com/kubeedge/ianvs/pull/827#issuecomment-5386637841> | HTTP 200 | api route |
| #875 | <https://github.com/user-attachments/assets/3edbe53f-3a60-4b96-83bc-7cc7708fe821> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #875 | <https://github.com/user-attachments/assets/5d124796-8cc2-4627-8504-56986334d71f> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #875 | <https://github.com/user-attachments/assets/915f6ab0-9565-407f-8ff6-c8c957150ba4> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #875 | <https://github.com/user-attachments/assets/c1212a54-c37e-42e4-9558-37b9ae769e24> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #875 | <https://youtu.be/VJqXbzr75cs> | unresolved TLS transport failure | video playback not independently verified from sandbox |
| #877 | <https://github.com/kubeedge/ianvs/issues/367> | HTTP 200 | api route |
| #877 | <https://github.com/kubeedge/ianvs/issues/369> | HTTP 200 | api route |
| #877 | <https://github.com/kubeedge/ianvs/issues/423> | HTTP 200 | api route |
| #882 | <https://github.com/kubeedge/sedna.git#subdirectory=lib> | HTTP 200 | web route |
| #882 | <https://github.com/user-attachments/assets/1754757d-86f7-4a7c-a1cf-679ef9934959> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #882 | <https://github.com/user-attachments/assets/1d359b09-13f7-4a92-8f03-c2a8cdba54ea> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #882 | <https://github.com/user-attachments/assets/218d454f-e4d0-45a9-be14-e5e69a5c8dd9> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #882 | <https://github.com/user-attachments/assets/40070943-ad86-47fe-85d7-f491437f294b> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #882 | <https://github.com/user-attachments/assets/44d1ccfd-2dfe-4756-91a1-974be2dcd3fd> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #882 | <https://github.com/user-attachments/assets/487b583d-4c8f-4bfb-9512-f549f758da7b> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #882 | <https://github.com/user-attachments/assets/682c114d-403f-44a3-9981-e9d539a9f2d5> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #882 | <https://github.com/user-attachments/assets/8a240ba4-612a-4b42-8f6a-5d51521abb8b> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #882 | <https://github.com/user-attachments/assets/a6ba3b26-91c8-4d63-90c8-6c207f2ec3c8> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #882 | <https://github.com/user-attachments/assets/afedfa7f-af3c-4f74-bf03-51d42c7cc550> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #882 | <https://github.com/user-attachments/assets/d5812211-6f93-48d7-8c53-70c3f945022b> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #882 | <https://github.com/user-attachments/assets/e8910d7d-0eb7-4ccf-a184-f92c17ea26d6> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #882 | <https://github.com/user-attachments/assets/ffd760fd-2b54-4e6a-8432-3f2f8ccba9aa> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #886 | <https://github.com/kubeedge/ianvs/issues/797> | HTTP 200 | api route |
| #886 | <https://github.com/kubeedge/ianvs/issues/798> | HTTP 200 | api route |
| #886 | <https://github.com/kubeedge/ianvs/pull/596> | HTTP 200 | api route |
| #886, #924 | <https://github.com/kubeedge/ianvs/pull/799> | HTTP 200 | api route |
| #886 | <https://github.com/kubeedge/ianvs/pull/800> | HTTP 200 | api route |
| #892, #923 | <https://github.com/kubeedge/ianvs> | HTTP 200 | web route |
| #894 | <https://github.com/user-attachments/assets/2a4f8196-c481-4bc9-8696-ef6409aab423> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #894 | <https://github.com/user-attachments/assets/2f893e15-9c8f-487c-8795-21d0c531725c> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #894 | <https://github.com/user-attachments/assets/56186305-3916-489b-9c0d-2037a95d25e8> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #894 | <https://github.com/user-attachments/assets/5a505a5a-d3f9-41f0-9ff2-63a2c2b4cfa9> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #894 | <https://github.com/user-attachments/assets/8d6002fa-9aef-4a7c-8b35-e11f61acc557> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #894 | <https://github.com/user-attachments/assets/8e11441f-9020-4946-a490-8cb580aff073> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #894 | <https://github.com/user-attachments/assets/8e485754-5553-4e8d-b389-69c30553c34b> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #894 | <https://github.com/user-attachments/assets/a7dbc5dd-05c8-4840-a901-fb28ebdf4520> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #894 | <https://github.com/user-attachments/assets/c90735e9-519b-4996-be70-815f1ffa2d90> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #894 | <https://github.com/user-attachments/assets/df315341-4569-4338-a297-97b1c84b8eb4> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #904 | <https://github.com/kubeedge/ianvs/blob/37a9c60a9747af0cfe3170f84249bc349c56e8d5/core/testcasecontroller/algorithm/algorithm.py#L78-L79> | HTTP 200 | api route |
| #904 | <https://github.com/kubeedge/ianvs/blob/37a9c60a9747af0cfe3170f84249bc349c56e8d5/core/testcasecontroller/algorithm/paradigm/base.py#L51-L57> | HTTP 200 | api route |
| #904 | <https://github.com/kubeedge/ianvs/blob/37a9c60a9747af0cfe3170f84249bc349c56e8d5/core/testcasecontroller/algorithm/paradigm/lifelong_learning/lifelong_learning.py#L263-L271> | HTTP 200 | api route |
| #904 | <https://github.com/kubeedge/ianvs/blob/37a9c60a9747af0cfe3170f84249bc349c56e8d5/core/testcasecontroller/algorithm/paradigm/lifelong_learning/lifelong_learning.py#L352-L369> | HTTP 200 | api route |
| #904 | <https://github.com/kubeedge/ianvs/blob/37a9c60a9747af0cfe3170f84249bc349c56e8d5/core/testcasecontroller/testcase/testcase.py#L54-L75> | HTTP 200 | api route |
| #904 | <https://github.com/kubeedge/ianvs/blob/37a9c60a9747af0cfe3170f84249bc349c56e8d5/core/testcasecontroller/testcasecontroller.py#L46-L61> | HTTP 200 | api route |
| #904 | <https://github.com/kubeedge/ianvs/blob/37a9c60a9747af0cfe3170f84249bc349c56e8d5/examples/Cloud_Robotics/singletask_learning_bench/Semantic_Segmentation/testalgorithms/basemodel.py#L66-L70> | HTTP 200 | api route |
| #904 | <https://github.com/kubeedge/ianvs/blob/37a9c60a9747af0cfe3170f84249bc349c56e8d5/examples/RoboDK%20Palletizing/singletask_learning_bench/testalgorithms/basemodel.py#L74-L77> | HTTP 200 | api route |
| #904 | <https://github.com/kubeedge/ianvs/blob/37a9c60a9747af0cfe3170f84249bc349c56e8d5/examples/cityscapes/singletask_learning_bench/semantic-segmentation/testalgorithms/rfnet/basemodel.py#L66-L70> | HTTP 200 | api route |
| #904 | <https://github.com/kubeedge/ianvs/blob/37a9c60a9747af0cfe3170f84249bc349c56e8d5/examples/robot-cityscapes-synthia/lifelong_learning_bench/semantic-segmentation/testalgorithms/erfnet/ERFNet/train.py#L26-L34> | HTTP 200 | api route |
| #904 | <https://github.com/kubeedge/ianvs/blob/37a9c60a9747af0cfe3170f84249bc349c56e8d5/examples/robot-cityscapes-synthia/lifelong_learning_bench/semantic-segmentation/testalgorithms/erfnet/ERFNet/utils/saver.py#L9-L19> | HTTP 200 | api route |
| #904 | <https://github.com/kubeedge/ianvs/blob/4519213d4db6b393c252130fbdbbd5156b88ad58/examples/robot-cityscapes-synthia/lifelong_learning_bench/semantic-segmentation/testalgorithms/erfnet/ERFNet/utils/saver.py#L9-L20> | HTTP 200 | api route |
| #904 | <https://github.com/kubeedge/ianvs/blob/8b6e4aadb16509fc03fafa448931092ff290897e/core/testcasecontroller/algorithm/paradigm/singletask_learning/singletask_learning.py#L118-L128> | HTTP 200 | api route |
| #904 | <https://github.com/kubeedge/ianvs/blob/8b6e4aadb16509fc03fafa448931092ff290897e/core/testcasecontroller/algorithm/paradigm/singletask_learning/singletask_learning.py#L49-L52> | HTTP 200 | api route |
| #904 | <https://github.com/kubeedge/ianvs/commit/37a9c60a9747af0cfe3170f84249bc349c56e8d5> | HTTP 200 | api route |
| #904 | <https://github.com/kubeedge/ianvs/commit/4519213d4db6b393c252130fbdbbd5156b88ad58> | HTTP 200 | api route |
| #904 | <https://github.com/kubeedge/ianvs/commit/8b6e4aadb16509fc03fafa448931092ff290897e> | HTTP 200 | api route |
| #904 | <https://github.com/kubeedge/ianvs/commit/96522f6b14fcfd408f2f9d3c7e94fc1bbde829ce> | HTTP 200 | api route |
| #904 | <https://github.com/kubeedge/ianvs/discussions/856> | HTTP 200 | api route |
| #904 | <https://github.com/kubeedge/ianvs/discussions/866> | HTTP 200 | api route |
| #904 | <https://github.com/kubeedge/ianvs/discussions/875> | HTTP 200 | api route |
| #904 | <https://github.com/kubeedge/ianvs/discussions/904#discussioncomment-18132202> | resolved against fetched Discussion comment | snapshot/API comment record |
| #904 | <https://github.com/kubeedge/ianvs/discussions/904#discussioncomment-18132311> | resolved against fetched Discussion comment | snapshot/API comment record |
| #904 | <https://github.com/kubeedge/ianvs/discussions/904#discussioncomment-18132387> | resolved against fetched Discussion comment | snapshot/API comment record |
| #904 | <https://github.com/kubeedge/ianvs/discussions/904#discussioncomment-18132533> | resolved against fetched Discussion comment | snapshot/API comment record |
| #904 | <https://github.com/kubeedge/ianvs/issues/457> | HTTP 200 | api route |
| #904 | <https://github.com/kubeedge/ianvs/issues/457#issuecomment-4577595480> | HTTP 200 | api route |
| #904 | <https://github.com/kubeedge/ianvs/issues/457#issuecomment-4577607369> | HTTP 200 | api route |
| #904 | <https://github.com/kubeedge/ianvs/issues/467> | HTTP 200 | api route |
| #904 | <https://github.com/kubeedge/ianvs/issues/467#issuecomment-5392087458> | HTTP 200 | api route |
| #904 | <https://github.com/kubeedge/ianvs/issues/572> | HTTP 200 | api route |
| #904 | <https://github.com/kubeedge/ianvs/issues/572#issuecomment-5392047215> | HTTP 200 | api route |
| #904 | <https://github.com/kubeedge/ianvs/issues/574> | HTTP 200 | api route |
| #904 | <https://github.com/kubeedge/ianvs/issues/574#issuecomment-5392071829> | HTTP 200 | api route |
| #904 | <https://github.com/kubeedge/ianvs/pull/309> | HTTP 200 | api route |
| #904 | <https://github.com/kubeedge/ianvs/pull/416> | HTTP 200 | api route |
| #904 | <https://github.com/kubeedge/ianvs/pull/573> | HTTP 200 | api route |
| #904 | <https://github.com/kubeedge/ianvs/pull/573#discussion_r3511196539> | HTTP 200 | api route |
| #904 | <https://github.com/kubeedge/ianvs/pull/573#pullrequestreview-5005572789> | HTTP 200 | api route |
| #904 | <https://github.com/kubeedge/ianvs/pull/575> | HTTP 200 | api route |
| #904 | <https://github.com/kubeedge/ianvs/pull/575#pullrequestreview-5005607801> | HTTP 200 | api route |
| #904 | <https://github.com/kubeedge/ianvs/pull/593> | HTTP 200 | api route |
| #904 | <https://github.com/kubeedge/ianvs/pull/593#pullrequestreview-5005624625> | HTTP 200 | api route |
| #904 | <https://github.com/kubeedge/sedna/blob/2c30e569eedadf6a89c22f3ec6d6310a61c3b66b/lib/sedna/common/config.py#L291-L301> | HTTP 200 | web route |
| #904 | <https://github.com/kubeedge/sedna/blob/2c30e569eedadf6a89c22f3ec6d6310a61c3b66b/lib/sedna/core/lifelong_learning/lifelong_learning.py#L241-L249> | HTTP 200 | web route |
| #904 | <https://github.com/user-attachments/assets/0f8f798d-3712-4143-9d7b-99e33b392307> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #904 | <https://github.com/user-attachments/assets/3abe7c69-9ae8-442a-ae98-76df5b553314> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #904 | <https://github.com/user-attachments/assets/751e18cf-9416-451c-b0db-cee0d3a67504> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #904 | <https://github.com/user-attachments/assets/82822c4a-0dc5-4e85-a191-a9c52081e159> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #904 | <https://github.com/user-attachments/assets/bd32aa19-3c4f-400e-a3d7-e09a241006f3> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #904 | <https://github.com/user-attachments/assets/cc959c36-968f-4dc9-8479-5895417a6140> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #906 | <https://github.com/user-attachments/assets/ae6c0576-5549-453c-a28c-286223b7ccda> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #906 | <https://github.com/user-attachments/assets/d29199e2-2f3b-4cdc-abbd-b83568859aac> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #906 | <https://github.com/user-attachments/assets/f505f330-d752-4a3f-b677-8b47bcc0cb8f> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #907 | <https://github.com/user-attachments/assets/7a046350-1c6c-4a98-97ff-c9fdde186d06> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #907 | <https://github.com/user-attachments/assets/8b0c0151-85f8-4d39-94f9-42041729a22c> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #907 | <https://github.com/user-attachments/assets/93b89131-16b6-4d84-b1b0-3918b391c564> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #907 | <https://github.com/user-attachments/assets/d453d29d-3c2a-49d3-8fad-32935ba47822> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #911 | <https://github.com/ifzhang/ByteTrack.git> | HTTP 200 | web route |
| #915 | <https://github.com/ChiragSW> | HTTP 200 | api route |
| #915 | <https://github.com/kubeedge/ianvs/issues/353> | HTTP 200 | api route |
| #915 | <https://github.com/kubeedge/ianvs/issues/440> | HTTP 200 | api route |
| #915 | <https://github.com/kubeedge/ianvs/pull/426> | HTTP 200 | api route |
| #915 | <https://github.com/kubeedge/ianvs/pull/429> | HTTP 200 | api route |
| #916 | <https://github.com/blackdragoon26> | HTTP 200 | api route |
| #916 | <https://github.com/blackdragoon26/ianvs/tree/e007aa01887fd3345c2d59e1bcedf0855bbaa56d/docs/lfx-term3-pretest-evidence> | HTTP 200 | web route |
| #916 | <https://github.com/kubeedge/ianvs/blob/176fad4cc863f8df28e6a880488d2da757661b43/core/testcasecontroller/algorithm/paradigm/federated_learning/federated_learning.py#L78-L85> | HTTP 200 | api route |
| #916 | <https://github.com/kubeedge/ianvs/blob/67b59e2b38ebaf10d70d40a40d8071f5bde694d4/examples/RoboDK%20Palletizing/singletask_learning_bench/testalgorithms/basemodel.py#L270-L294> | HTTP 200 | api route |
| #916 | <https://github.com/kubeedge/ianvs/blob/67b59e2b38ebaf10d70d40a40d8071f5bde694d4/examples/RoboDK%20Palletizing/singletask_learning_bench/testenv/map50.py#L83-L105> | HTTP 200 | api route |
| #916 | <https://github.com/kubeedge/ianvs/blob/b07ca3872a3fb26b238f5e40d903f4ae2a8155a7/examples/GovDoc2Poster/singletask_learning_bench/testalgorithms/gen/basemodel.py#L454-L462> | HTTP 200 | api route |
| #916 | <https://github.com/kubeedge/ianvs/blob/b07ca3872a3fb26b238f5e40d903f4ae2a8155a7/examples/GovDoc2Poster/singletask_learning_bench/testalgorithms/gen/gov_parser.py#L61-L78> | HTTP 200 | api route |
| #916 | <https://github.com/kubeedge/ianvs/discussions/916#discussioncomment-18141305> | resolved against fetched Discussion comment | snapshot/API comment record |
| #916 | <https://github.com/kubeedge/ianvs/discussions/916#discussioncomment-18141306> | resolved against fetched Discussion comment | snapshot/API comment record |
| #916 | <https://github.com/kubeedge/ianvs/discussions/916#discussioncomment-18141309> | resolved against fetched Discussion comment | snapshot/API comment record |
| #916 | <https://github.com/kubeedge/ianvs/discussions/916#discussioncomment-18141310> | resolved against fetched Discussion comment | snapshot/API comment record |
| #916 | <https://github.com/kubeedge/ianvs/discussions/916#discussioncomment-18141311> | resolved against fetched Discussion comment | snapshot/API comment record |
| #916 | <https://github.com/kubeedge/ianvs/issues/230> | HTTP 200 | api route |
| #916 | <https://github.com/kubeedge/ianvs/issues/565> | HTTP 200 | api route |
| #916 | <https://github.com/kubeedge/ianvs/issues/565#issuecomment-5402924117> | HTTP 200 | api route |
| #916 | <https://github.com/kubeedge/ianvs/issues/700> | HTTP 200 | api route |
| #916 | <https://github.com/kubeedge/ianvs/issues/700#issuecomment-5402926288> | HTTP 200 | api route |
| #916, #924 | <https://github.com/kubeedge/ianvs/issues/731> | HTTP 200 | api route |
| #916 | <https://github.com/kubeedge/ianvs/issues/731#issuecomment-5402926778> | HTTP 200 | api route |
| #916 | <https://github.com/kubeedge/ianvs/pull/566> | HTTP 200 | api route |
| #916 | <https://github.com/kubeedge/ianvs/pull/566#issuecomment-5402927232> | HTTP 200 | api route |
| #916 | <https://github.com/kubeedge/ianvs/pull/649> | HTTP 200 | api route |
| #916 | <https://github.com/kubeedge/ianvs/pull/649#issuecomment-5402928804> | HTTP 200 | api route |
| #916 | <https://github.com/kubeedge/ianvs/pull/705> | HTTP 200 | api route |
| #916 | <https://github.com/kubeedge/ianvs/pull/705#issuecomment-5402927792> | HTTP 200 | api route |
| #916 | <https://github.com/kubeedge/ianvs/pull/721> | HTTP 200 | api route |
| #916 | <https://github.com/kubeedge/ianvs/pull/721#issuecomment-5402929773> | HTTP 200 | api route |
| #916, #924 | <https://github.com/kubeedge/ianvs/pull/736> | HTTP 200 | api route |
| #916 | <https://github.com/kubeedge/ianvs/pull/736#issuecomment-5402928294> | HTTP 200 | api route |
| #916 | <https://github.com/kubeedge/ianvs/pull/770> | HTTP 200 | api route |
| #916 | <https://github.com/kubeedge/ianvs/pull/770#issuecomment-5402929291> | HTTP 200 | api route |
| #916 | <https://raw.githubusercontent.com/blackdragoon26/ianvs/e007aa01887fd3345c2d59e1bcedf0855bbaa56d/docs/lfx-term3-pretest-evidence/pretest-contract-transcript.png> | GitHub Contents API 200 | raw transport failed; blob independently exists at pinned ref/path |
| #917 | <https://github.com/user-attachments/assets/6d021db6-55a6-408e-8638-994c7b604a51> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #917 | <https://github.com/user-attachments/assets/9b660f3a-1596-47a2-97da-a729962ddb68> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #921 | <https://github.com/kubeedge/ianvs/commit/30a757f147d30c8197b00fcc408e07200319a748> | HTTP 200 | api route |
| #921 | <https://github.com/kubeedge/ianvs/commit/360947f92392b9f7c17e325c89951041ca49ef29> | HTTP 200 | api route |
| #921 | <https://github.com/kubeedge/ianvs/commit/aa2a5b22d31c22eef74629d04edc86d21b020dd2> | HTTP 200 | api route |
| #921 | <https://github.com/kubeedge/ianvs/discussions/921#discussioncomment-18145877> | resolved against fetched Discussion comment | snapshot/API comment record |
| #921 | <https://github.com/kubeedge/ianvs/discussions/921#discussioncomment-18146164> | resolved against fetched Discussion comment | snapshot/API comment record |
| #921 | <https://github.com/kubeedge/ianvs/discussions/921#discussioncomment-18146279> | resolved against fetched Discussion comment | snapshot/API comment record |
| #921 | <https://github.com/kubeedge/ianvs/discussions/921#discussioncomment-18146380> | resolved against fetched Discussion comment | snapshot/API comment record |
| #921 | <https://github.com/kubeedge/ianvs/discussions/921#discussioncomment-18146738> | resolved against fetched Discussion comment | snapshot/API comment record |
| #921 | <https://github.com/kubeedge/ianvs/issues/639> | HTTP 200 | api route |
| #921 | <https://github.com/kubeedge/ianvs/issues/639#issuecomment-5408089451> | HTTP 200 | api route |
| #921 | <https://github.com/kubeedge/ianvs/issues/842#issuecomment-5408109234> | HTTP 200 | api route |
| #921 | <https://github.com/kubeedge/ianvs/pull/415> | HTTP 200 | api route |
| #921 | <https://github.com/kubeedge/ianvs/pull/418> | HTTP 200 | api route |
| #921 | <https://github.com/kubeedge/ianvs/pull/418#pullrequestreview-5017104349> | HTTP 200 | api route |
| #921 | <https://github.com/kubeedge/ianvs/pull/640> | HTTP 200 | api route |
| #921 | <https://github.com/kubeedge/ianvs/pull/640#issuecomment-5409706034> | HTTP 200 | api route |
| #921 | <https://github.com/kubeedge/ianvs/pull/640#issuecomment-5409854387> | HTTP 200 | api route |
| #921 | <https://github.com/kubeedge/ianvs/pull/640#pullrequestreview-5017084071> | HTTP 200 | api route |
| #921 | <https://github.com/kubeedge/ianvs/pull/664#issuecomment-5409274633> | HTTP 200 | api route |
| #921 | <https://github.com/kubeedge/ianvs/pull/664#pullrequestreview-5017826825> | HTTP 200 | api route |
| #921 | <https://github.com/kubeedge/ianvs/pull/682#pullrequestreview-5017718521> | HTTP 200 | api route |
| #921 | <https://github.com/kubeedge/ianvs/pull/683#pullrequestreview-5017497591> | HTTP 200 | api route |
| #921 | <https://github.com/kubeedge/ianvs/pull/800#pullrequestreview-5017796787> | HTTP 200 | api route |
| #921 | <https://github.com/kubeedge/ianvs/pull/803#pullrequestreview-5017121592> | HTTP 200 | api route |
| #921 | <https://github.com/kubeedge/ianvs/pull/843#issuecomment-5409927217> | HTTP 200 | api route |
| #921 | <https://github.com/kubeedge/ianvs/pull/843#issuecomment-5410068771> | HTTP 200 | api route |
| #921 | <https://github.com/kubeedge/ianvs/pull/843#pullrequestreview-5017143488> | HTTP 200 | api route |
| #921 | <https://github.com/user-attachments/assets/760628bd-b929-447c-a4c5-7f542b9723be> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #921 | <https://github.com/user-attachments/assets/b8533ab3-fcee-4e4e-b70c-fbdc64508c94> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #921 | <https://github.com/user-attachments/assets/f12f3216-4185-4156-9b4b-813c1e7e2eea> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #922 | <https://github.com/user-attachments/assets/0b0f9d8c-f173-49d5-b3a6-0b8417b652d9> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #922 | <https://github.com/user-attachments/assets/dbae0c5b-23be-4164-932c-17a3e2b3f015> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #923 | <https://github.com/user-attachments/assets/62aa29d2-e52a-4700-9541-c97918f97f4d> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #923 | <https://github.com/user-attachments/assets/b6bc1686-c77b-4761-b875-d60267593ea3> | HTTP 302 route; binary fetch TLS-failed | asset URL exists at redirect layer; content not downloaded in sandbox |
| #924 | <https://github.com/kubeedge/ianvs/issues/626> | HTTP 200 | api route |
| #924 | <https://github.com/kubeedge/ianvs/issues/637> | HTTP 200 | api route |
| #924 | <https://github.com/kubeedge/ianvs/pull/627> | HTTP 200 | api route |
| #924 | <https://github.com/kubeedge/ianvs/pull/638> | HTTP 200 | api route |
| #924 | <https://github.com/kubeedge/ianvs/pull/776> | HTTP 200 | api route |
| #924 | <https://github.com/kubeedge/ianvs/pull/780> | HTTP 200 | api route |
| #924 | <https://github.com/kubeedge/ianvs/pull/783> | HTTP 200 | api route |
| #924 | <https://github.com/kubeedge/ianvs/pull/784> | HTTP 200 | api route |
| #924 | <https://github.com/kubeedge/ianvs/pull/787> | HTTP 200 | api route |
| #924 | <https://github.com/kubeedge/ianvs/pull/790> | HTTP 200 | api route |
| #924 | <https://github.com/kubeedge/ianvs/pull/792> | HTTP 200 | api route |
| #924 | <https://github.com/kubeedge/ianvs/pull/811> | HTTP 200 | api route |
| #924 | <https://github.com/kubeedge/ianvs/pull/818> | HTTP 200 | api route |
| #924 | <https://github.com/kubeedge/ianvs/pull/848> | HTTP 200 | api route |
| #924 | <https://raw.githubusercontent.com/akshhkaushik/akshhkaushik/codex/ianvs-lfx-pretest-evidence/ianvs-lfx-2026-term-3/task1-core-baseline.png> | GitHub Contents API 200 | raw transport failed; blob independently exists at pinned ref/path |
| #924 | <https://raw.githubusercontent.com/akshhkaushik/akshhkaushik/codex/ianvs-lfx-pretest-evidence/ianvs-lfx-2026-term-3/task1-robodk-baseline.png> | GitHub Contents API 200 | raw transport failed; blob independently exists at pinned ref/path |
