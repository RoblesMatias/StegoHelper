## Summary
Simple helper to hide text into an image using LSB steganography. It also supports AES-256 encryption to enforce security of hidden text.

## Example
```csharp
using Cocona;
using StegoHelper;

namespace SteganoConsole
{
    class Program
    {
        static void Main(string[] args)
        {
            CoconaLiteApp.Run<Program>(args);
        }

        public void Hide([Option('i')] string inputImage, [Option('o')] string outputImage, [Option('t')] string textFile, [Option('p')] string password = "")
        {
            if (File.Exists(inputImage) && File.Exists(textFile))
            {
                using (StreamReader sr = File.OpenText(textFile))
                {
                    string text = sr.ReadToEnd();

                    if (!string.IsNullOrEmpty(password))
                        text = AESEncryptionHelper.Encrypt(text, password);

                    try
                    {
                        SteganographyHelper.EmbedText(text, inputImage, outputImage);
                    }
                    catch (InsufficientPixelsException ex)
                    {
                        Console.WriteLine(ex.Message);
                    }
                }
            }
        }

        public void Unhide([Option('i')] string inputImage, [Option('p')] string password = "")
        {
            if (File.Exists(inputImage))
            {
                string text = SteganographyHelper.ExtractText(inputImage);

                if (!string.IsNullOrEmpty(password))
                    text = AESEncryptionHelper.Decrypt(text, password);

                File.WriteAllText("secret.txt", text);
            }
            else
                Console.WriteLine("File not found");
        }
    }
}

```
## Videotutorial - C# Steganography Step by Step (Spanish)
[![](https://img.youtube.com/vi/-mU5D37Istw/0.jpg)](https://www.youtube.com/watch?v=-mU5D37Istw)
