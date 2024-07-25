
# Luma AI Genie Unofficial C# Library

Luma AI Genie is a C# library for interacting with the Luma Labs API. It allows users to create 3d models using text prompt. This library uses public methods of the Luma Genie API and does not attempt to hack or bypass any security measures.

## Disclaimer

This library is provided as-is, without any guarantees or warranties. The use of this library is at your own risk, and the authors are not responsible for any damages or issues that arise from its use.

## Installation

Clone the repository and include the source files in your project.

## Usage

### Initializing the Client

```csharp
using LumaGenie;

var client = new GenieClient("your_token_here");
```

### Updating the Token

```csharp
client.UpdateToken("new_token_here");
```

### Request to creating a model

```csharp
using LumaGenie.Data.Creation;

var creationRequest = new CreationRequest("your prompt here");

var creationResponse = await client.CreationRequestAsync(creationRequest);

if (creationResponse != null)
{
    string uuid = creationResponse.Response[0];
    Console.WriteLine("Creation successful! UUID: " + uuid);
}
```

### Getting Creation Status

```csharp
var statusResponse = await client.GetStatusAsync(uuid);

if (statusResponse != null)
{
    Console.WriteLine("Job status: " + statusResponse.Response.Status);
}
```

### Request to converting a model

```csharp
using LumaGenie.Data.Convert;

var convertRequest = new ConvertRequest(uuid, ExportFormat.Fbx);

var convertResponse = await client.ConvertRequestAsync(convertRequest);

if (convertResponse != null)
{
    string fileUrl = convertResponse.Response.UploadedFiles[0].FileUrl;
    Console.WriteLine("Conversion successful! FileURL: " + fileUrl);
}
```

### Download files

```csharp
await client.DownloadAndExtractFileAsync(fileUrl, OnFileExtracted);

private void OnFileExtracted(string fullName, Stream stream)
{
    string fileExtension = Path.GetExtension(fullName);
    string fileName = Path.GetFileNameWithoutExtension(fullName);

    if (fileExtension == ".fbx")
    {
        // Extract fbx file from Stream here
    }
    else if (fileExtension == ".jpg")
    {
        if (fileName.EndsWith("texture_kd"))
            // Extract color map from Stram here
        else if (fileName.EndsWith("roughness"))
            // Extract roughness map from Stram here
        else if (fileName.EndsWith("metallic"))
            // Extract metallic map from Stram here
    }
}
```

### Obtaining the Token

You can manually obtain the token through your browser. For example, in Chrome:

1. Open the Developer Console (press `F12`).
2. Navigate to the `Application` tab.
3. Go to `Cookies`.
4. Look for the `refreshToken` field.

Alternatively, you can implement OAuth authorization through the browser in your application. This platform-dependent implementation is not included in this library.

Use this token to initialize the `GenieClient`.

## Contributing

Feel free to open issues or submit pull requests for improvements and bug fixes.

## License

This project is licensed under the MIT License.
