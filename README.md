# Bluesky MCP Server

[![LightNow capabilities](https://lightnow.ai/badge/io.github.semioz/bluesky-mcp)](https://lightnow.ai/servers/io.github.semioz/bluesky-mcp)

A Model Context Protocol (MCP) server for Bluesky that can post on your behalf by using the AT Protocol.

## Setup

### Installing via Smithery

To install bluesky-mcp for Claude Desktop automatically via [Smithery](https://smithery.ai/server/@semioz/bluesky-mcp):

```bash
npx -y @smithery/cli install @semioz/bluesky-mcp --client claude
```

### Manual Installation

Configure Claude for Desktop:

Open your Claude for Desktop App configuration at `~/Library/Application Support/Claude/claude_desktop_config.json` and add:

```json
{
    "mcpServers": {
        "bluesky-mcp": {
            "command": "npx",
            "args": ["-y", "@semihberkay/bluesky-mcp"],
            "env": {
                "BLUESKY_IDENTIFIER": "your.handle.bsky.social",
                "BLUESKY_PASSWORD": "your-app-password"
            }
        }
    }
}
```

### Required Environment Variables
- `BLUESKY_IDENTIFIER`: Your Bluesky handle or email
- `BLUESKY_PASSWORD`: Your Bluesky app password

## Available MCP Tools

### Authentication
```typescript
tool: "login"
params: {
  identifier?: string, // Your Bluesky handle or email (optional if set in env)
  password?: string    // Your Bluesky app password (optional if set in env)
}
```
The server will attempt to auto-login using credentials from the environment variables when starting up. You only need to use the login tool if:
- You haven't set the environment variables in Claude's config
- You want to login with different credentials
- The auto-login failed

### Posts
```typescript
// Create a new post
tool: "create-post"
params: {
  text: string,           // The text content of your post
  images?: {              // Optional array of images
    data: string,         // Base64 encoded image data
    encoding: string      // Image MIME type (e.g., image/jpeg)
  }[]
}

// Get a single post
tool: "get-post"
params: {
  uri: string            // The URI of the post to fetch
}

// Get multiple posts
tool: "get-posts"
params: {
  uris: string[]        // Array of post URIs to fetch
}

// Delete a post
tool: "delete-post"
params: {
  uri: string           // The URI of the post to delete
}
```

### Interactions
```typescript
// Like a post
tool: "like-post"
params: {
  uri: string,          // The URI of the post to like
  cid: string          // The CID of the post to like
}

// Unlike a post
tool: "unlike-post"
params: {
  likeUri: string      // The URI of the like to remove
}

// Repost
tool: "repost"
params: {
  uri: string,         // The URI of the post to repost
  cid: string         // The CID of the post to repost
}

// Remove repost
tool: "unrepost"
params: {
  repostUri: string   // The URI of the repost to remove
}
```

### Profile & Timeline
```typescript
// Get your profile
tool: "get-profile"
params: {}

// Get timeline
tool: "get-timeline"
params: {
  limit?: number      // Number of posts to fetch (max 100)
}
```

## Prompts

### Format Timeline
```typescript
prompt: "format-timeline"
params: {
  timeline: any       // Timeline data to format
}
```
Formats timeline data in a human-readable way with:
- Author name/handle
- Post text
- Engagement metrics (replies, reposts, likes)
- Timestamps
- Embedded content (links, videos)
- Repost information

## Features

- ✅ Authentication with Bluesky
- ✅ Create text posts
- ✅ Support for image uploads
- ✅ Get user profile
- ✅ Get timeline
- ✅ Like/Unlike posts
- ✅ Repost/Unrepost
- ✅ Rich text support with automatic link and mention detection
- ✅ MCP compatible for use with Claude
