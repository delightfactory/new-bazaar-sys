# Media and Asset Policy

## Buckets

- Private product source images.
- Public approved marketplace media.
- Private receipts and exports.
- Public organizer and bazaar media after approval.

## Rules

- Private by default.
- Object paths include workspace ownership and non-guessable identifiers.
- File type, size, and image dimensions are validated.
- Public media is published by copying/promoting an approved asset reference, not by making the private source bucket public.
- Storage access is protected by RLS; upsert policies include required select/update permissions.
- Deleting a product archives references first; object deletion is asynchronous and auditable.
- Generate responsive variants/thumbnails outside core request transactions.
- Strip unsafe metadata where practical before public delivery.

