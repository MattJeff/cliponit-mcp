# ClipOnIt MCP Server

> Turn a long video into short, subtitled, auto-reframed clips — and make AI explainer videos from a topic or a voice-over — driven from Claude, ChatGPT, or any MCP client. 70 tools.

[ClipOnIt](https://cliponit.app) is a hosted video clipping service. This repository documents its **remote MCP server**, so that an AI assistant can run both products on your behalf:

- **Clipping** — import a video, find the best moments, reframe on faces, add word-by-word subtitles, and export vertical clips.
- **Explainer videos** — go from an idea, a script or a voice-over to an animated explainer built from a library of nearly 11,000 elements, then critique, polish and render it.

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

OAuth 2.1 with PKCE (S256). Clients identify themselves with a Client ID Metadata Document (an HTTPS `client_id` URL) or register through Dynamic Client Registration (RFC 7591) — no manual key needed. An API key (`Authorization: Bearer ck_…`, created from the billing page) works too for scripts. API and MCP access are included from the Starter plan.

## Tools

| Area | Tools |
|---|---|
| Videos | `list_videos` · `upload_video_file` · `import_video_url` · `get_video` · `wait_video` · `delete_video` |
| Clips | `find_moment` · `get_clip` · `delete_clip` · `set_clip_bounds` · `snap_clip_to_words` · `get_transcript` |
| Editing | `update_clip_edit` · `set_default_subtitles` · `auto_reframe` · `auto_zoom` · `detect_silences` · `describe_clip` |
| Export | `export_clip` · `export_all_clips` · `get_export` · `wait_export` · `cancel_export` · `get_export_frame` |
| Account | `get_quota` |
| Explainer — plan | `explainer_idea` · `explainer_storyboard` · `explainer_styles` · `explainer_templates` · `explainer_schema` · `explainer_check` |
| Explainer — elements | `explainer_library` · `explainer_browse` · `explainer_search_elements` · `explainer_suggest_elements` · `explainer_describe_elements` · `explainer_library_elements` · `explainer_my_elements` · `explainer_create_element` · `explainer_library_add_element` · `explainer_forge` |
| Explainer — build | `explainer_compose` · `explainer_produce` · `explainer_assist` · `explainer_variants` · `explainer_critique` · `explainer_polish` · `explainer_preview` · `explainer_versions` |
| Explainer — voice | `explainer_import_audio` · `explainer_audio_status` · `explainer_audio_list` · `explainer_compose_from_audio` · `explainer_narrate` · `explainer_retake` |
| Explainer — render | `explainer_render` · `explainer_wait` · `explainer_export` · `explainer_job_status` · `explainer_job_wait` · `explainer_jobs` · `explainer_job_cancel` |
| Explainer — library | `explainer_list` · `explainer_delete` · `explainer_my_templates` · `explainer_template_save` · `explainer_template_run` · `explainer_taste_get` · `explainer_brand_get` · `explainer_brand_set` |

Formats: 9:16, 16:9, 1:1 and 4:5, each with its own independent edit. Subtitles come in five styles, word by word.

## Example

> "Take my last uploaded video, find the 5 best moments, reframe them on the speaker, add yellow karaoke subtitles, and export them in 9:16."

The assistant chains `list_videos` → `get_video` → `auto_reframe` → `update_clip_edit` → `export_clip` → `wait_export` and hands back the MP4 links.

> "Make a 60-second explainer about how our onboarding works, in our brand colors, with a voice-over."

The assistant chains `explainer_idea` → `explainer_produce` → `explainer_job_wait` → `explainer_critique` → `explainer_polish` → `explainer_render` → `explainer_wait`.

## Links

- Web app — https://cliponit.app
- API reference (OpenAPI 3.1) — https://cliponit.app/openapi.json · interactive: https://cliponit.app/docs
- Plain-text summary for LLMs — https://cliponit.app/llms.txt
- Discovery manifest — https://cliponit.app/.well-known/mcp.json

## License

The contents of this repository (documentation and server manifest) are released under the MIT License. The ClipOnIt service itself is a hosted product; see https://cliponit.app/terms.
