---
permalink: /draft/17
title: 17/URL data
parent: Draft specs
layout: default
---

# 17/URL data

> Version: 0.1
>
> Status: Draft
>
> Authors: Felicio Mununga <felicio@status.im>

## Abstract

This document describes serialization, compression and encoding of data within URLs, so the app links could carry enough information to help users with instant previewing and verifying even prior clicking or taking an action on the content.

## Related scope

### Specs

- See <https://github.com/status-im/specs/pull/159> for URL scheme

### Features

- Onboarding website
- Link preview
- Link sharing
- Deep linking
- Routing and navigation

### App entities

- Community
- Channel
- User

## Requirements

### Product

- Encode data within URL until Waku response is instant and considered reliable enough for link previewing and visiting of the onboarding website. Or until a product roadmap for alternative decentralized storage is finalized.
- A verify state while loading onboarding website until encoded data is matched against Waku response to mitigate identity attacks
- Accept only `[A-Za-z0-9_-.\u0020]` character class for textual fields
- Shortest result possible

## Implementation

### Example

- See <https://github.com/status-im/status-web/pull/345/files>

### Data

#### Fields

```protobuf
syntax = "proto3";

message Community {
 string display_name = 1;
 string description = 2;
 uint32 members_count = 3;
 string color = 4;
 repeated uint32 tag_indices = 5;
}

message Channel {
 string display_name = 1;
 string description = 2;
 string emoji = 3;
 string color = 4;
 Community community = 5;
 string uuid = 6;
}

message User {
 string display_name = 1;
 string description = 2;
 string color = 3;
}

message URLData {
 // Community, Channel, or User
 bytes content = 1;
}

// Field on CommunityDescription, CommunityChat and ContactCodeAdvertisement
message URLParams {
 string encoded_url_data = 1;
 // Signature of encoded URL data
 string encoded_signature = 2;
}
```

### Encoding

- Base64url

### Compression

- Brotli

### Serialization

- Protocol buffers

## Proposals

- See <https://docs.google.com/spreadsheets/d/1JD4kp0aUm90piUZ7FgM_c2NGe2PdN8BFB11wmt5UZIY/edit?usp=sharing> for all
- See <https://docs.google.com/spreadsheets/d/1JD4kp0aUm90piUZ7FgM_c2NGe2PdN8BFB11wmt5UZIY/edit#gid=1895477181> for final two

## Discussions

- See <https://github.com/status-im/status-web/issues/327>

## Proof of concept

- See <https://github.com/felicio/status-web/blob/825262c4f07a68501478116c7382862607a5544e/packages/status-js/src/utils/encode-url-data.compare.test.ts#L4>

## Footnotes

- This specification prefers maintainability, extensibility, project familiarity and backward compatibility.
- Implementation and proposals are most effective for mid to max field lengths, not min.
- Mind that some social platforms or IMs visually trim the URLs, so they would actually not be displayed in full length. See <https://docs.google.com/spreadsheets/d/1JD4kp0aUm90piUZ7FgM_c2NGe2PdN8BFB11wmt5UZIY/edit#gid=1260088614>.
- Without sharing public keys with hosting servers some data like profile photos or banners cannot be included in the previews
