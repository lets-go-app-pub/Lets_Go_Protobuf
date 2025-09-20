# Lets_Go_Protobuf — shared gRPC/Protobuf contracts

Canonical `.proto` files for the Lets Go platform. These schemas define the wire contracts used by the **Android client**, **Desktop Admin (Qt)**, and the **C++ Server** (plus internal admin tools). Kept in a standalone repo so all components consume the **same** source of truth.

> **Scope:** message types and RPC surfaces for auth, matching, chat, admin/ops, requests, errors, and utilities.

---

## What’s here (folders)

- **`proto_client_server_shared/`** — contracts shared by **end-user clients and server**  
  - Auth/session: `LoginFunction.proto`, `LoginSupportFunctions.proto`, `LoginToServerBasicInfo.proto`, `LoginValuesToReturnToClient.proto`, `PreLoginTimestamps.proto`, `AccountState.proto`, `AccountLoginTypeEnum.proto`, `AccessStatusEnum.proto`, `UserAccountType.proto`, `UserSubscriptionStatus.proto`  
  - Matching/discovery: `FindMatches.proto`, `UserMatchOptions.proto`, `AlgorithmSearchOptions.proto`, `CategoryTimeFrame.proto`, `LetsGoEventStatus.proto`  
  - Chat & rooms: `ChatMessageStream.proto`, `TypeOfChatMessage.proto`, `ChatRoomCommands.proto`, `ChatRoomInfoMessage.proto`, `ChatMessageToClientMessage.proto`, `CreatedChatRoomInfo.proto`, `MemberSharedInfoMessage.proto`, `UpdateOtherUserMessages.proto`  
  - Requests & fields: `RequestMessages.proto`, `RequestFields.proto`, `EventRequestMessage.proto`, `RetrieveServerLoad.proto`  
  - Notifications & verification: `SMSVerification.proto`, `EmailSendingMessages.proto`  
  - Errors & reporting: `ErrorMessage.proto`, `ErrorOriginEnum.proto`, `ReportMessages.proto`, `SendErrorToServer.proto`  
  - Status enums: `StatusEnum.proto`

- **`proto_server_specific/`** — **admin/ops-only** contracts used by the server and Desktop Admin  
  - Admin commands: `ManageServerCommands.proto`, `AdminChatRoomCommands.proto`, `AdminEventCommands.proto`, `SetAdminFields.proto`  
  - Admin reads: `RequestAdminInfo.proto`, `RequestUserAccountInfo.proto`, `RequestStatistics.proto`  
  - Moderation & health: `HandleErrors.proto`, `HandleReports.proto`, `HandleFeedback.proto`, `ErrorHandledMoveReasonEnum.proto`, `UserAccountStatusEnum.proto`  
  - Catalog & matching enums: `AccountCategoryEnum.proto`, `MatchTypeEnum.proto`  
  - Utilities/testing: `SendPictureForTesting.proto`  
  - Privilege levels: `AdminLevelEnum.proto`

- **`obsolete/`** — superseded or historical files (retained for context)

---

## Design notes

- **Clear split of concerns:** end-user surfaces live in `proto_client_server_shared/`; privileged/ops surfaces in `proto_server_specific/`.  
- **Additive evolution:** new fields use new tag numbers; existing tags are never repurposed. Deprecated fields are **reserved** to avoid reuse.  
- **Stable enums:** enums prefer explicit numeric values; changes are append-only.  
- **Streaming where it matters:** chat uses server-driven streaming; admin endpoints are mostly unary/iterators for predictability.  
- **Compatibility target:** Android/Kotlin, C++ (server), and auxiliary Kotlin/Qt tools generate from the same protos.

---

## Versioning / compatibility

- **Proto syntax:** `proto3`.  
- **Tag safety:** never reuse or renumber tags; mark removed fields with `reserved`.  
- **Backwards compatible changes:** add fields with sensible defaults; avoid changing wire types.  
- **Breaking changes:** if unavoidable, introduce a **new message** and migrate callers gradually.

---

## Related repositories

- **Server (C++)** — stateless hub, gRPC/Protobuf, MongoDB  
  👉 [`Lets_Go_Server`](https://github.com/lets-go-app-pub/Lets_Go_Server)

- **Android Client (Kotlin)** — auth, profiles, activities, chat *(SDK versions may be dated)*  
  👉 [`Lets_Go_Android_Client`](https://github.com/lets-go-app-pub/Lets_Go_Android_Client)

- **Desktop Admin (Qt)** — admin/ops console for moderation, events, stats, and controls  
  👉 [`Lets_Go_Interface`](https://github.com/lets-go-app-pub/Lets_Go_Interface)

---

## License

See `LICENSE` in this repo. Note that **generated code** inherits licensing from these `.proto` sources and your generator/runtime libraries.
