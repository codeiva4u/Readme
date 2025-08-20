# Puppeteer Real Browser MCP Server with Playwright Support: विस्तृत गाइड

## प्रोजेक्ट क्या है और यह कैसे काम करता है?

### परिचय
Puppeteer Real Browser MCP Server एक Model Context Protocol (MCP) सर्वर है जो AI सहायकों (जैसे Claude) को एक वास्तविक वेब ब्राउज़र को नियंत्रित करने की क्षमता प्रदान करता है। यह ZFC Digital के puppeteer-real-browser पैकेज पर आधारित है, जो बॉट पहचान से बचने के लिए डिज़ाइन किया गया है।

### मुख्य विशेषताएं
- **डिफ़ॉल्ट रूप से स्टील्थ**: सभी ब्राउज़र इंस्टेंस एंटी-डिटेक्शन सुविधाएं उपयोग करते हैं
- **विंडोज़ समर्थन**: विस्तृत Chrome पहचान और ECONNREFUSED त्रुटि सुधार (v1.3.0+)
- **उन्नत कॉन्फ़िगरेशन**: puppeteer-real-browser विकल्पों के लिए पूर्ण समर्थन
- **डायनेमिक सेलेक्टर खोज**: हार्डकोडेड सेलेक्टर के बिना बुद्धिमान तत्व खोज
- **कैप्चा हैंडलिंग**: reCAPTCHA, hCaptcha और Turnstile के लिए समर्थन
- **रॉबस्ट त्रुटि हैंडलिंग**: सर्किट ब्रेकर पैटर्न के साथ उन्नत त्रुटि पुनर्प्राप्ति
- **स्टैक ओवरफ्लो सुरक्षा**: अनंत पुनरावृत्ति के खिलाफ व्यापक सुरक्षा
- **Playwright समर्थन**: स्वचालित ब्राउज़र प्रबंधन और क्रॉस-प्लेटफ़ॉर्म संगतता

### कैसे काम करता है?
यह सर्वर MCP प्रोटोकॉल को लागू करता है, जिससे AI सहायक एक वास्तविक ब्राउज़र को नियंत्रित कर सकते हैं, सामग्री निकाल सकते हैं, और अधिक कर सकते हैं। यह puppeteer-real-browser और Playwright का उपयोग करके बॉट पहचान तंत्रों को बायपास करने की क्षमता प्रदान करता है।

## विंडोज़ मशीन पर सेटअप

### आवश्यकताएं
1. Node.js >= 18.0.0
2. npm या yarn
3. Google Chrome या Chromium ब्राउज़र स्थापित (puppeteer के लिए)
4. Windows 10/11

### सेटअप चरण

#### 1. Node.js स्थापित करें
- [nodejs.org](https://nodejs.org/) पर जाएं
- Node.js (संस्करण 18 या उच्चतर) डाउनलोड और स्थापित करें
- स्थापना की पुष्टि करें: `node --version`

#### 2. Claude Desktop कॉन्फ़िगर करें
1. फ़ाइल एक्सप्लोरर खोलें और नेविगेट करें: `%APPDATA%\Claude\`
2. `claude_desktop_config.json` खोलें (या बनाएं)
3. निम्न कॉन्फ़िगरेशन जोड़ें:

```json
{
  "mcpServers": {
    "puppeteer-real-browser": {
      "command": "npx",
      "args": ["puppeteer-real-browser-mcp-server@latest"]
    }
  }
}
```

#### 3. Claude Desktop पुनरारंभ करें
Claude Desktop को पूरी तरह से बंद करें और फिर से खोलें।

#### 4. परीक्षण करें
Claude Desktop में कहें:
> "Initialize a browser and navigate to google.com, then get the page content"

### विंडोज़-विशिष्ट Chrome पथ
विंडोज़ (v1.3.0+) में, सर्वर स्वचालित रूप से Chrome स्थापना पथों का पता लगाता है:
- मानक स्थापनाएं: `C:\Program Files\Google\Chrome\Application\chrome.exe`
- 32-बिट स्थापनाएं: `C:\Program Files (x86)\Google\Chrome\Application\chrome.exe`
- उपयोगकर्ता स्थापनाएं: `%LOCALAPPDATA%\Google\Chrome\Application\chrome.exe`
- Chrome Canary: `%LOCALAPPDATA%\Google\Chrome SxS\Application\chrome.exe`
- पोर्टेबल स्थापनाएं और रजिस्ट्री-पहचाने गए पथ

## लिनक्स मशीन में सेटअप और उपयोग

### आवश्यकताएं
1. Node.js >= 18.0.0
2. npm या yarn
3. Google Chrome या Chromium ब्राउज़र स्थापित (puppeteer के लिए)
4. headless संचालन के लिए xvfb

### सेटअप चरण

#### 1. आवश्यक पैकेज स्थापित करें
```bash
# Chrome/Chromium स्थापित करें
sudo apt-get install -y google-chrome-stable

# headless संचालन के लिए xvfb स्थापित करें
sudo apt-get install -y xvfb
```

#### 2. Claude Code CLI के साथ उपयोग
```bash
# MCP सर्वर जोड़ें
claude mcp add puppeteer-real-browser -- npx puppeteer-real-browser-mcp-server@latest

# परीक्षण करें
claude
# फिर कहें: "Initialize a browser and navigate to google.com, then get the page content"
```

#### 3. मैनुअल कॉन्फ़िगरेशन
कॉन्फ़िगरेशन फ़ाइल स्थान:
- प्रोजेक्ट-विशिष्ट: अपने प्रोजेक्ट निर्देशिका में `.cursor/mcp.json` बनाएं
- वैश्विक: अपने होम निर्देशिका में `~/.cursor/mcp.json` बनाएं

बेसिक कॉन्फ़िगरेशन:
```json
{
  "mcpServers": {
    "puppeteer-real-browser": {
      "command": "npx",
      "args": ["puppeteer-real-browser-mcp-server@latest"]
    }
  }
}
```

## Playwright समर्थन के साथ सभी डिवाइसों में ब्राउज़र का उपयोग

### Playwright क्या है?
Playwright एक शक्तिशाली ब्राउज़र ऑटोमेशन लाइब्रेरी है जो Microsoft द्वारा विकसित की गई है। यह Chromium, Firefox और WebKit के लिए एकल API प्रदान करती है।

### Playwright के फायदे

#### 1. स्वचालित ब्राउज़र डाउनलोड
- Playwright स्वचालित रूप से आवश्यक ब्राउज़र डाउनलोड करता है
- कोई भी मैनुअल Chrome/Chromium स्थापना की आवश्यकता नहीं है
- सभी प्लेटफॉर्म पर समान अनुभव

#### 2. क्रॉस-प्लेटफ़ॉर्म समर्थन
- **Windows**: x86, x64, ARM64
- **macOS**: Intel और Apple Silicon (ARM64)
- **Linux**: x86, x64, ARM, ARM64
- **X Virtual Framebuffer (xvfb)** के साथ headless समर्थन

#### 3. बेहतर डिटेक्शन प्रतिरोध
- स्वचालित एंटी-डिटेक्शन पैच
- बेहतर stealth मोड
- कैप्चा हैंडलिंग के लिए बेहतर समर्थन

#### 4. बेहतर प्रदर्शन
- तेज़ पेज लोडिंग
- कम मेमोरी उपयोग
- बेहतर नेटवर्क प्रबंधन

### Playwright का उपयोग कैसे करें

#### 1. Playwright समर्थन सक्षम करें
ब्राउज़र प्रारंभ करते समय `browserType` पैरामीटर का उपयोग करें:

```
User: "Initialize a browser with Playwright support"
AI: I'll initialize a browser using Playwright for better cross-platform compatibility.
```

#### 2. कॉन्फ़िगरेशन विकल्प
```json
{
  "browserType": "playwright",
  "headless": false
}
```

#### 3. Playwright के साथ ब्राउज़र प्रारंभ करना
```typescript
// Playwright स्वचालित रूप से ब्राउज़र डाउनलोड और प्रबंधित करता है
import { chromium } from 'playwright';

const browser = await chromium.launch({
  headless: false,
  args: [
    '--no-first-run',
    '--no-default-browser-check',
    '--disable-blink-features=AutomationControlled'
  ]
});
```

## सभी डिवाइसों में ब्राउज़र का उपयोग

### समर्थित प्लेटफ़ॉर्म
यह प्रोजेक्ट निम्नलिखित प्लेटफ़ॉर्म पर काम करता है:
- **Windows**: x86, x64, ARM64
- **macOS**: Intel और Apple Silicon (ARM64)
- **Linux**: x86, x64, ARM, ARM64

### प्लेटफ़ॉर्म-विशिष्ट विचार

#### Windows
- स्वचालित Chrome पथ पहचान (15+ स्थानों की खोज करता है)
- रजिस्ट्री-आधारित पहचान
- Windows-विशिष्ट Chrome फ्लैग्स
- सुधारित कनेक्शन प्रतिधारण

#### macOS
- Chrome के लिए `/Applications/Google Chrome.app/` में खोज
- Apple Silicon (ARM64) और Intel (x86_64) समर्थन
- स्वचालित पथ पहचान

#### Linux
- कई स्थानों की खोज: `/usr/bin/google-chrome`, `/usr/bin/chromium-browser`
- Snap स्थापनाओं के लिए समर्थन
- X Virtual Framebuffer (xvfb) के साथ headless समर्थन
- ARM और ARM64 समर्थन

### क्रॉस-प्लेटफ़ॉर्म संगतता
प्रोजेक्ट package.json में निम्नलिखित के साथ क्रॉस-प्लेटफ़ॉर्म समर्थन की घोषणा करता है:
```json
"os": [
  "darwin",
  "linux",
  "win32"
]
```

यह सुनिश्चित करता है कि पैकेज सभी प्रमुख ऑपरेटिंग सिस्टम पर स्थापित हो सकता है।

### ब्राउज़र संगतता
- **Google Chrome**: पूर्ण समर्थन (puppeteer के साथ)
- **Chromium**: पूर्ण समर्थन (puppeteer और Playwright दोनों के साथ)
- **Chrome Canary**: विंडोज़ और macOS पर समर्थन
- **Microsoft Edge**: सीमित समर्थन (कुछ कार्यों के लिए)

### Playwright के साथ क्रॉस-प्लेटफ़ॉर्म संगतता
Playwright के साथ, सभी प्लेटफॉर्म पर स्वचालित संगतता प्रदान की जाती है:
- कोई मैनुअल Chrome पथ कॉन्फ़िगरेशन की आवश्यकता नहीं
- कोई प्लेटफ़ॉर्म-विशिष्ट कोड की आवश्यकता नहीं
- स्वचालित एंटी-डिटेक्शन पैच

### उपयोग के उदाहरण

#### बुनियादी वेब ब्राउज़िंग
```
User: "Initialize a browser and navigate to example.com"
AI: I'll initialize a stealth browser and navigate to the website.
```

#### Playwright के साथ ब्राउज़िंग
```
User: "Initialize a browser with Playwright and navigate to example.com"
AI: I'll initialize a browser using Playwright for better cross-platform compatibility and navigate to the website.
```

#### फॉर्म स्वचालन
```
User: "Fill in the search form with 'test query'"
AI: I'll type that into the search field.
```

#### डेटा निष्कर्षण
```
User: "Get all the product names from this e-commerce page"
AI: I'll extract the product information from the page.
```

### समस्या निवारण

#### सामान्य मुद्दे
1. **"Chrome not found" त्रुटियां**: स्पष्ट Chrome पथ निर्दिष्ट करें
2. **ECONNREFUSED त्रुटियां**: विंडोज़ होस्ट्स फ़ाइल की जांच करें
3. **परमिशन डिनाइड त्रुटियां**: sudo के साथ चलाएं (Linux/macOS) या एडमिनिस्ट्रेटर के रूप में चलाएं (Windows)

#### प्लेटफ़ॉर्म-विशिष्ट समाधान
- **Windows**: Chrome प्रक्रियाओं को मारें, एडमिनिस्ट्रेटर के रूप में चलाएं
- **Linux**: xvfb स्थापित करें, परमिशन जांचें
- **macOS**: Chrome को एप्लिकेशन फ़ोल्डर में रखें

#### Playwright के साथ समस्या निवारण
1. **ब्राउज़र डाउनलोड में विफलता**: `npx playwright install chromium` चलाएं
2. **मेमोरी समस्याएं**: headless मोड का उपयोग करें
3. **डिटेक्शन मुद्दे**: stealth मोड सक्षम करें

## निष्कर्ष
Puppeteer Real Browser MCP Server with Playwright Support एक शक्तिशाली टूल है जो AI सहायकों को वास्तविक ब्राउज़र को नियंत्रित करने की क्षमता प्रदान करता है। Playwright समर्थन के साथ, यह सभी प्लेटफॉर्म पर स्वचालित संगतता प्रदान करता है और बेहतर प्रदर्शन और डिटेक्शन प्रतिरोध प्रदान करता है।

यह विस्तृत गाइड आपको Puppeteer Real Browser MCP Server with Playwright Support को किसी भी समर्थित प्लेटफॉर्म पर स्थापित और उपयोग करने में मदद करने के लिए डिज़ाइन की गई है।
