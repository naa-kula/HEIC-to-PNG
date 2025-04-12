# HEIC-to-PNG
HEIC to PNG converter
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>HEIC to PNG Converter (Multiple Files)</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f6f6f6;
      margin: 0;
      padding: 2rem;
      display: flex;
      flex-direction: column;
      align-items: center;
    }
    h1 {
      color: #333;
    }
    .converter {
      background: #fff;
      padding: 2rem;
      border-radius: 8px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.15);
      max-width: 400px;
      width: 100%;
      text-align: center;
    }
    input[type="file"] {
      margin: 1rem 0;
    }
    button {
      background: #007BFF;
      color: #fff;
      border: none;
      padding: 0.6rem 1.2rem;
      border-radius: 4px;
      cursor: pointer;
    }
    button:hover {
      background: #0056b3;
    }
    #message {
      margin-top: 1rem;
      font-style: italic;
      color: #555;
      text-align: left;
      max-height: 200px;
      overflow-y: auto;
    }
  </style>
</head>
<body>
  <div class="converter">
    <h1>HEIC to PNG Converter</h1>
    <!-- Allowing multiple file selection -->
    <input type="file" id="heic-input" accept=".heic, .HEIC, image/heic, image/heif" multiple>
    <br>
    <button id="convert-btn">Convert</button>
    <div id="message"></div>
  </div>

  <!-- Include the heic2any library -->
  <script src="https://unpkg.com/heic2any@0.3.0/dist/heic2any.min.js"></script>
  <script>
    const convertBtn = document.getElementById("convert-btn");
    const fileInput = document.getElementById("heic-input");
    const messageDiv = document.getElementById("message");

    convertBtn.addEventListener("click", async () => {
      const files = fileInput.files;
      if (!files || files.length === 0) {
        messageDiv.textContent = "Please select at least one HEIC file to convert.";
        return;
      }

      messageDiv.innerHTML = "Starting conversion...<br>";

      // Process each selected file sequentially
      for (const file of files) {
        messageDiv.innerHTML += `Converting ${file.name}...<br>`;
        try {
          const convertedBlob = await heic2any({
            blob: file,
            toType: "image/png"
          });
          
          // Create a downloadable link for the PNG file
          const url = URL.createObjectURL(convertedBlob);
          const link = document.createElement("a");
          // Replace .heic (or .HEIC) extension with .png
          const newFileName = file.name.replace(/\.heic$/i, ".png");
          link.href = url;
          link.download = newFileName;
          document.body.appendChild(link);
          link.click();
          document.body.removeChild(link);
          
          messageDiv.innerHTML += `${file.name} conversion complete!<br>`;
        } catch (error) {
          console.error("Error converting", file.name, error);
          messageDiv.innerHTML += `Error converting ${file.name}.<br>`;
        }
      }

      messageDiv.innerHTML += "All conversions complete.";
    });
  </script>
</body>
</html>
