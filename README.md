[![MseeP.ai Security Assessment Badge](https://mseep.net/pr/mikeyny-ai-image-gen-mcp-badge.png)](https://mseep.ai/app/mikeyny-ai-image-gen-mcp)

# Image Generation MCP Server

An [MCP (Model Context Protocol)](https://modelcontextprotocol.io/) server implementation for generating images using Replicate's [`black-forest-labs/flux-schnell`](https://replicate.com/black-forest-labs/flux-schnell) model.

Ideally to be used with Cursor's MCP feature, but can be used with any MCP client.

## Features

- Generate images from text prompts
- Configurable image parameters (resolution, aspect ratio, quality)
- Save generated images to specified directory
- Full MCP protocol compliance
- Error handling and validation

## Prerequisites

- Node.js 16+
- Replicate API token
- TypeScript SDK for MCP

## Setup

1. Clone the repository
2. Install dependencies:
   ```bash
   npm install
   ```
3. Add your Replicate API token directly in the code at `src/imageService.ts` by updating the `apiToken` constant:
   ```bash
   // No environment variables are used since they can't be easily set in cursor
   const apiToken = "your-replicate-api-token-here";
   ```

   > **Note:** If using with Claude, you can create a `.env` file in the root directory and set your API token there:
   ```bash
   REPLICATE_API_TOKEN=your-replicate-api-token-here
   ```

   Then build the project:
   ```bash
   npm run build
   ```

## Usage

To use with cursor:
1. Go to Settings
2. Select Features
3. Scroll down to "MCP Servers"
4. Click "Add new MCP Server"
5. Set Type to "Command"
6. Set Command to: `node ./path/to/dist/server.js`

## API Parameters

| Parameter           | Type    | Required | Default | Description                                     |
|--------------------|---------|----------|---------|------------------------------------------------|
| `prompt`           | string  | Yes      | -       | Text prompt for image generation               |
| `output_dir`       | string  | Yes      | -       | Server directory path to save generated images |
| `go_fast`          | boolean | No       | false   | Enable faster generation mode                  |
| `megapixels`       | string  | No       | "1"     | Resolution quality ("1", "2", "4")            |
| `num_outputs`      | number  | No       | 1       | Number of images to generate (1-4)            |
| `aspect_ratio`     | string  | No       | "1:1"   | Aspect ratio ("1:1", "4:3", "16:9")          |
| `output_format`    | string  | No       | "webp"  | Image format ("webp", "png", "jpeg")         |
| `output_quality`   | number  | No       | 80      | Compression quality (1-100)                   |
| `num_inference_steps`| number| No       | 4       | Number of denoising steps (4-20)             |

## Example Request

```json
{
  "prompt": "black forest gateau cake spelling out 'FLUX SCHNELL'",
  "output_dir": "/var/output/images",
  "filename": "black_forest_cake",
  "output_format": "webp"
  "go_fast": true,
  "megapixels": "1",
  "num_outputs": 2,
  "aspect_ratio": "1:1"
}
```

## Example Response

```json
{
  "image_paths": [
    "/var/output/images/output_0.webp",
    "/var/output/images/output_1.webp"
  ],
  "metadata": {
    "model": "black-forest-labs/flux-schnell",
    "inference_time_ms": 2847
  }
}
```

## Error Handling

The server handles the following error types:

- Validation errors (invalid parameters)
- API errors (Replicate API issues)
- Server errors (filesystem, permissions)
- Unknown errors (unexpected issues)

Each error response includes:
- Error code
- Human-readable message
- Detailed error information

## License

ISC 