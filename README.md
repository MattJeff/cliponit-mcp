# ClipOnIt MCP Server

> Turn a long video into short, subtitled, auto-reframed clips — driven from Claude, ChatGPT, or any MCP client.

[ClipOnIt](https://cliponit.app) is a hosted video clipping service. This repository documents its **remote MCP server**, so that an AI assistant can run the whole pipeline on your behalf: import a video, find the best moments, reframe on faces, add word-by-word subtitles, and export vertical clips.

**Endpoint:** `https://cliponit.app/mcp` (Streamable HTTP)

## Connect

**Claude.ai / Claude Desktop** — Settings → Connectors → Add custom connector → paste `https://cliponit.app/mcp` → Connect → sign in → Authorize.

**Claude Code**
```sh
claude mcp add --transport http cliponit https://cliponit.app/mcp
```
Then `/mcp` → Authenticate.

**ChatGPT** — Settings → Connectors → Developer mode → Create → URL `https://cliponit.app/mcp` → OAuth.

**Anything else** — the server speaks JSON-RPC 2.0 over HTTP POST and advertises OAuth 2.1 metadata at `/.well-known/oauth-protected-resource`.

## Authentication

OAuth 2.1 with Dynamic Client Registration (RFC 7591) and PKCE (S256) — no manual key needed; your client registers itself. An API key (`Authorization: Bearer ck_…`, created from the billing page) works too for scripts. API and MCP access are included from the Starter plan.

## Tools

| Area | Tools |
|---|---|
| Videos | `list_videos` · `upload_video_file` · `import_video_url` · `get_video` · `wait_video` · `delete_video` |
| Clips | `find_moment` · `get_clip` · `delete_clip` · `set_clip_bounds` · `snap_clip_to_words` · `get_transcript` |
| Editing | `update_clip_edit` · `set_default_subtitles` · `auto_reframe` · `auto_zoom` · `detect_silences` · `describe_clip` |
| Export | `export_clip` · `export_all_clips` · `get_export` · `wait_export` · `cancel_export` · `get_export_frame` |
| Account | `get_quota` |

Formats: 9:16, 16:9, 1:1 and 4:5, each with its own independent edit. Subtitles come in five styles, word by word.

## Example

> "Take my last uploaded video, find the 5 best moments, reframe them on the speaker, add yellow karaoke subtitles, and export them in 9:16."

The assistant chains `list_videos` → `get_video` → `auto_reframe` → `update_clip_edit` → `export_clip` → `wait_export` and hands back the MP4 links.

## Links

- Web app — https://cliponit.app
- API reference (OpenAPI 3.1) — https://cliponit.app/openapi.json · interactive: https://cliponit.app/docs
- Plain-text summary for LLMs — https://cliponit.app/llms.txt
- Discovery manifest — https://cliponit.app/.well-known/mcp.json

## License

The contents of this repository (documentation and server manifest) are released under the MIT License. The ClipOnIt service itself is a hosted product; see https://cliponit.app/terms.
