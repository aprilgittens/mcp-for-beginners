<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "fa635ae747c9b4d5c2f61c6c46cb695f",
  "translation_date": "2025-07-17T18:15:03+00:00",
  "source_file": "03-GettingStarted/01-first-server/README.md",
  "language_code": "ne"
}
-->
# MCP सँग सुरु गर्दै

Model Context Protocol (MCP) सँग तपाईंको पहिलो कदममा स्वागत छ! तपाईं MCP मा नयाँ हुनुहुन्छ वा आफ्नो बुझाइलाई गहिरो बनाउन चाहनुहुन्छ भने, यो मार्गदर्शकले आवश्यक सेटअप र विकास प्रक्रियामा तपाईंलाई सहयोग गर्नेछ। तपाईंले कसरी MCP ले AI मोडेलहरू र अनुप्रयोगहरू बीच सहज एकीकरण सक्षम बनाउँछ भन्ने पत्ता लगाउनुहुनेछ, र कसरी छिटो आफ्नो वातावरण तयार पारेर MCP-संचालित समाधानहरू निर्माण र परीक्षण गर्ने सिक्नुहुनेछ।

> TLDR; यदि तपाईं AI अनुप्रयोगहरू बनाउनुहुन्छ भने, तपाईंलाई थाहा छ कि तपाईंले आफ्नो LLM (ठूलो भाषा मोडेल) मा उपकरणहरू र अन्य स्रोतहरू थप्न सक्नुहुन्छ, जसले LLM लाई अझ ज्ञानवान बनाउँछ। तर यदि ती उपकरणहरू र स्रोतहरू सर्भरमा राख्नु भयो भने, अनुप्रयोग र सर्भर क्षमताहरू कुनै पनि क्लाइन्टले LLM सहित वा बिना प्रयोग गर्न सक्छ।

## अवलोकन

यस पाठले MCP वातावरणहरू सेटअप गर्ने र तपाईंका पहिलो MCP अनुप्रयोगहरू निर्माण गर्ने व्यावहारिक मार्गदर्शन प्रदान गर्दछ। तपाईंले आवश्यक उपकरण र फ्रेमवर्कहरू कसरी सेटअप गर्ने, आधारभूत MCP सर्भरहरू कसरी बनाउने, होस्ट अनुप्रयोगहरू कसरी सिर्जना गर्ने, र तपाईंका कार्यान्वयनहरू कसरी परीक्षण गर्ने सिक्नुहुनेछ।

Model Context Protocol (MCP) एउटा खुला प्रोटोकल हो जसले अनुप्रयोगहरूले LLMs लाई सन्दर्भ कसरी प्रदान गर्ने भन्ने कुरा मानकीकृत गर्छ। MCP लाई AI अनुप्रयोगहरूको लागि USB-C पोर्ट जस्तै सोच्नुहोस् - यसले AI मोडेलहरूलाई विभिन्न डेटा स्रोतहरू र उपकरणहरूसँग जडान गर्ने मानकीकृत तरिका प्रदान गर्छ।

## सिकाइका उद्देश्यहरू

यस पाठको अन्त्यसम्म, तपाईं सक्षम हुनुहुनेछ:

- C#, Java, Python, TypeScript, र JavaScript मा MCP को लागि विकास वातावरण सेटअप गर्न
- कस्टम सुविधाहरू (स्रोतहरू, प्रॉम्प्टहरू, र उपकरणहरू) सहित आधारभूत MCP सर्भरहरू निर्माण र तैनाथ गर्न
- MCP सर्भरहरूसँग जडान हुने होस्ट अनुप्रयोगहरू सिर्जना गर्न
- MCP कार्यान्वयनहरू परीक्षण र डिबग गर्न

## तपाईंको MCP वातावरण सेटअप गर्दै

MCP सँग काम सुरु गर्नु अघि, तपाईंको विकास वातावरण तयार पार्नु र आधारभूत कार्यप्रवाह बुझ्नु महत्त्वपूर्ण छ। यस खण्डले तपाईंलाई सुरुवाती सेटअपका चरणहरूमा मार्गदर्शन गर्नेछ ताकि MCP सँग सहज सुरुवात गर्न सकियोस्।

### पूर्वआवश्यकताहरू

MCP विकासमा डुब्नु अघि, सुनिश्चित गर्नुहोस् कि तपाईं सँग छ:

- **विकास वातावरण**: तपाईंले रोजेको भाषा (C#, Java, Python, TypeScript, वा JavaScript) को लागि
- **IDE/संपादक**: Visual Studio, Visual Studio Code, IntelliJ, Eclipse, PyCharm, वा कुनै आधुनिक कोड संपादक
- **प्याकेज प्रबन्धकहरू**: NuGet, Maven/Gradle, pip, वा npm/yarn
- **API कुञ्जीहरू**: तपाईंले होस्ट अनुप्रयोगहरूमा प्रयोग गर्ने कुनै पनि AI सेवाहरूका लागि

## आधारभूत MCP सर्भर संरचना

एक MCP सर्भर सामान्यतया समावेश गर्दछ:

- **सर्भर कन्फिगरेसन**: पोर्ट, प्रमाणीकरण, र अन्य सेटिङहरू सेटअप गर्ने
- **स्रोतहरू**: LLMs लाई उपलब्ध गराइने डेटा र सन्दर्भ
- **उपकरणहरू**: मोडेलहरूले कल गर्न सक्ने कार्यक्षमता
- **प्रॉम्प्टहरू**: पाठ सिर्जना वा संरचना गर्नका लागि टेम्प्लेटहरू

यहाँ TypeScript मा एक सरल उदाहरण छ:

```typescript
import { Server, Tool, Resource } from "@modelcontextprotocol/typescript-server-sdk";

// Create a new MCP server
const server = new Server({
  port: 3000,
  name: "Example MCP Server",
  version: "1.0.0"
});

// Register a tool
server.registerTool({
  name: "calculator",
  description: "Performs basic calculations",
  parameters: {
    expression: {
      type: "string",
      description: "The math expression to evaluate"
    }
  },
  handler: async (params) => {
    const result = eval(params.expression);
    return { result };
  }
});

// Start the server
server.start();
```

माथिको कोडमा हामीले:

- MCP TypeScript SDK बाट आवश्यक कक्षाहरू आयात गरेका छौं।
- नयाँ MCP सर्भर उदाहरण सिर्जना र कन्फिगर गरेका छौं।
- कस्टम उपकरण (`calculator`) लाई ह्यान्डलर फंक्शनसहित दर्ता गरेका छौं।
- MCP अनुरोधहरू सुन्न सर्भर सुरु गरेका छौं।

## परीक्षण र डिबगिङ

तपाईंको MCP सर्भर परीक्षण सुरु गर्नु अघि, उपलब्ध उपकरणहरू र डिबगिङका लागि उत्तम अभ्यासहरू बुझ्नु महत्त्वपूर्ण छ। प्रभावकारी परीक्षणले तपाईंको सर्भरले अपेक्षित व्यवहार गर्छ भन्ने सुनिश्चित गर्छ र छिटो समस्याहरू पत्ता लगाउन र समाधान गर्न मद्दत गर्छ। तलको खण्डले तपाईंको MCP कार्यान्वयनलाई मान्य पार्न सिफारिस गरिएका तरिकाहरू वर्णन गर्दछ।

MCP ले तपाईंलाई सर्भरहरू परीक्षण र डिबग गर्न मद्दत गर्ने उपकरणहरू प्रदान गर्छ:

- **Inspector tool**, यो ग्राफिकल इन्टरफेसले तपाईंलाई सर्भरसँग जडान भएर उपकरणहरू, प्रॉम्प्टहरू र स्रोतहरू परीक्षण गर्न अनुमति दिन्छ।
- **curl**, तपाईं कमाण्ड लाइन उपकरण जस्तै curl वा HTTP आदेशहरू सिर्जना र चलाउन सक्ने अन्य क्लाइन्टहरू प्रयोग गरेर पनि सर्भरसँग जडान गर्न सक्नुहुन्छ।

### MCP Inspector प्रयोग गर्दै

[MCP Inspector](https://github.com/modelcontextprotocol/inspector) एउटा भिजुअल परीक्षण उपकरण हो जसले तपाईंलाई मद्दत गर्छ:

1. **सर्भर क्षमताहरू पत्ता लगाउने**: उपलब्ध स्रोतहरू, उपकरणहरू, र प्रॉम्प्टहरू स्वचालित रूपमा पत्ता लगाउने
2. **उपकरण कार्यान्वयन परीक्षण गर्ने**: विभिन्न प्यारामिटरहरू प्रयास गर्ने र वास्तविक समयमा प्रतिक्रिया हेर्ने
3. **सर्भर मेटाडाटा हेर्ने**: सर्भर जानकारी, स्किमाहरू, र कन्फिगरेसनहरू जाँच्ने

```bash
# ex TypeScript, installing and running MCP Inspector
npx @modelcontextprotocol/inspector node build/index.js
```

माथिका आदेशहरू चलाउँदा, MCP Inspector तपाईंको ब्राउजरमा स्थानीय वेब इन्टरफेस सुरु गर्नेछ। तपाईंले दर्ता गरिएका MCP सर्भरहरू, तिनीहरूको उपलब्ध उपकरणहरू, स्रोतहरू, र प्रॉम्प्टहरू देखिने ड्यासबोर्ड देख्नुहुनेछ। इन्टरफेसले तपाईंलाई अन्तरक्रियात्मक रूपमा उपकरण कार्यान्वयन परीक्षण गर्न, सर्भर मेटाडाटा निरीक्षण गर्न, र वास्तविक समय प्रतिक्रियाहरू हेर्न अनुमति दिन्छ, जसले तपाईंका MCP सर्भर कार्यान्वयनहरू मान्य र डिबग गर्न सजिलो बनाउँछ।

यहाँ यसको स्क्रिनशट छ:

![](../../../../translated_images/connected.73d1e042c24075d386cacdd4ee7cd748c16364c277d814e646ff2f7b5eefde85.ne.png)

## सामान्य सेटअप समस्याहरू र समाधानहरू

| समस्या | सम्भावित समाधान |
|-------|-------------------|
| कनेक्शन अस्वीकृत | सर्भर चलिरहेको छ कि छैन र पोर्ट सही छ कि छैन जाँच्नुहोस् |
| उपकरण कार्यान्वयन त्रुटिहरू | प्यारामिटर मान्यकरण र त्रुटि ह्यान्डलिङ समीक्षा गर्नुहोस् |
| प्रमाणीकरण असफलता | API कुञ्जीहरू र अनुमति जाँच्नुहोस् |
| स्किमा मान्यकरण त्रुटिहरू | प्यारामिटरहरू परिभाषित स्किमासँग मेल खान्छन् कि छैनन् सुनिश्चित गर्नुहोस् |
| सर्भर सुरु नहुनु | पोर्ट द्वन्द्व वा हराइरहेका निर्भरता जाँच्नुहोस् |
| CORS त्रुटिहरू | क्रस-ओरिजिन अनुरोधहरूको लागि उचित CORS हेडरहरू कन्फिगर गर्नुहोस् |
| प्रमाणीकरण समस्या | टोकन वैधता र अनुमति जाँच्नुहोस् |

## स्थानीय विकास

स्थानीय विकास र परीक्षणका लागि, तपाईंले MCP सर्भरहरू सिधै आफ्नो मेसिनमा चलाउन सक्नुहुन्छ:

1. **सर्भर प्रक्रिया सुरु गर्नुहोस्**: तपाईंको MCP सर्भर अनुप्रयोग चलाउनुहोस्
2. **नेटवर्किङ कन्फिगर गर्नुहोस्**: सर्भर अपेक्षित पोर्टमा पहुँचयोग्य छ कि छैन सुनिश्चित गर्नुहोस्
3. **क्लाइन्टहरू जडान गर्नुहोस्**: `http://localhost:3000` जस्ता स्थानीय कनेक्शन URL प्रयोग गर्नुहोस्

```bash
# Example: Running a TypeScript MCP server locally
npm run start
# Server running at http://localhost:3000
```

## तपाईंको पहिलो MCP सर्भर निर्माण गर्दै

हामीले पहिलेको पाठमा [मूल अवधारणाहरू](/01-CoreConcepts/README.md) कभर गरिसकेका छौं, अब त्यो ज्ञानलाई व्यवहारमा उतार्ने समय आएको छ।

### सर्भरले के गर्न सक्छ

कोड लेख्न सुरु गर्नु अघि, सर्भरले के गर्न सक्छ भन्ने कुरा सम्झौं:

MCP सर्भरले उदाहरणका लागि:

- स्थानीय फाइलहरू र डाटाबेसहरू पहुँच गर्न सक्छ
- रिमोट API हरूसँग जडान गर्न सक्छ
- गणना गर्न सक्छ
- अन्य उपकरण र सेवाहरूसँग एकीकरण गर्न सक्छ
- अन्तरक्रियाका लागि प्रयोगकर्ता इन्टरफेस प्रदान गर्न सक्छ

राम्रो, अब हामीलाई थाहा छ के गर्न सक्छ, कोड लेख्न सुरु गरौं।

## अभ्यास: सर्भर सिर्जना गर्दै

सर्भर सिर्जना गर्न, तपाईंले यी चरणहरू पालना गर्नुपर्छ:

- MCP SDK इन्स्टल गर्नुहोस्।
- एउटा प्रोजेक्ट सिर्जना गरी प्रोजेक्ट संरचना सेटअप गर्नुहोस्।
- सर्भर कोड लेख्नुहोस्।
- सर्भर परीक्षण गर्नुहोस्।

### -1- SDK इन्स्टल गर्नुहोस्

यो तपाईंले रोजेको रनटाइम अनुसार अलिक फरक हुन्छ, त्यसैले तलका रनटाइमहरू मध्ये एक रोज्नुहोस्:

> [!NOTE]
> Python को लागि, हामी पहिले प्रोजेक्ट संरचना सिर्जना गर्नेछौं र त्यसपछि निर्भरता इन्स्टल गर्नेछौं।

### TypeScript

```sh
npm install @modelcontextprotocol/sdk zod
npm install -D @types/node typescript
```

### Python

```sh
# Create project dir
mkdir calculator-server
cd calculator-server
# Open the folder in Visual Studio Code - Skip this if you are using a different IDE
code .
```

### .NET

```sh
dotnet new console -n McpCalculatorServer
cd McpCalculatorServer
```

### Java

Java को लागि, Spring Boot प्रोजेक्ट सिर्जना गर्नुहोस्:

```bash
curl https://start.spring.io/starter.zip \
  -d dependencies=web \
  -d javaVersion=21 \
  -d type=maven-project \
  -d groupId=com.example \
  -d artifactId=calculator-server \
  -d name=McpServer \
  -d packageName=com.microsoft.mcp.sample.server \
  -o calculator-server.zip
```

जिप फाइल निकाल्नुहोस्:

```bash
unzip calculator-server.zip -d calculator-server
cd calculator-server
# optional remove the unused test
rm -rf src/test/java
```

तपाईंको *pom.xml* फाइलमा निम्न पूर्ण कन्फिगरेसन थप्नुहोस्:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    
    <!-- Spring Boot parent for dependency management -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.5.0</version>
        <relativePath />
    </parent>

    <!-- Project coordinates -->
    <groupId>com.example</groupId>
    <artifactId>calculator-server</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>Calculator Server</name>
    <description>Basic calculator MCP service for beginners</description>

    <!-- Properties -->
    <properties>
        <java.version>21</java.version>
        <maven.compiler.source>21</maven.compiler.source>
        <maven.compiler.target>21</maven.compiler.target>
    </properties>

    <!-- Spring AI BOM for version management -->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.ai</groupId>
                <artifactId>spring-ai-bom</artifactId>
                <version>1.0.0-SNAPSHOT</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <!-- Dependencies -->
    <dependencies>
        <dependency>
            <groupId>org.springframework.ai</groupId>
            <artifactId>spring-ai-starter-mcp-server-webflux</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-starter-test</artifactId>
         <scope>test</scope>
      </dependency>
    </dependencies>

    <!-- Build configuration -->
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <release>21</release>
                </configuration>
            </plugin>
        </plugins>
    </build>

    <!-- Repositories for Spring AI snapshots -->
    <repositories>
        <repository>
            <id>spring-milestones</id>
            <name>Spring Milestones</name>
            <url>https://repo.spring.io/milestone</url>
            <snapshots>
                <enabled>false</enabled>
            </snapshots>
        </repository>
        <repository>
            <id>spring-snapshots</id>
            <name>Spring Snapshots</name>
            <url>https://repo.spring.io/snapshot</url>
            <releases>
                <enabled>false</enabled>
            </releases>
        </repository>
    </repositories>
</project>
```

### -2- प्रोजेक्ट सिर्जना गर्नुहोस्

अब तपाईंले SDK इन्स्टल गर्नुभयो, अब प्रोजेक्ट सिर्जना गरौं:

### TypeScript

```sh
mkdir src
npm install -y
```

### Python

```sh
# Create a virtual env and install dependencies
python -m venv venv
venv\Scripts\activate
pip install "mcp[cli]"
```

### Java

```bash
cd calculator-server
./mvnw clean install -DskipTests
```

### -3- प्रोजेक्ट फाइलहरू सिर्जना गर्नुहोस्  
### TypeScript

*package.json* निम्न सामग्रीसहित सिर्जना गर्नुहोस्:

```json
{
   "type": "module",
   "bin": {
     "weather": "./build/index.js"
   },
   "scripts": {
     "build": "tsc && node build/index.js"
   },
   "files": [
     "build"
   ]
}
```

*tsconfig.json* निम्न सामग्रीसहित सिर्जना गर्नुहोस्:

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "Node16",
    "moduleResolution": "Node16",
    "outDir": "./build",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules"]
}
```

### Python

*server.py* फाइल सिर्जना गर्नुहोस्  
```sh
touch server.py
```

### .NET

आवश्यक NuGet प्याकेजहरू इन्स्टल गर्नुहोस्:

```sh
dotnet add package ModelContextProtocol --prerelease
dotnet add package Microsoft.Extensions.Hosting
```

### Java

Java Spring Boot प्रोजेक्टहरूको लागि, प्रोजेक्ट संरचना स्वचालित रूपमा सिर्जना हुन्छ।

### -4- सर्भर कोड सिर्जना गर्नुहोस्

### TypeScript

*index.ts* फाइल सिर्जना गरी निम्न कोड थप्नुहोस्:

```typescript
import { McpServer, ResourceTemplate } from "@modelcontextprotocol/sdk/server/mcp.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";
import { z } from "zod";
 
// Create an MCP server
const server = new McpServer({
  name: "Demo",
  version: "1.0.0"
});
```

अब तपाईं सर्भर छ, तर यो धेरै काम गर्दैन, त्यसलाई सुधारौं।

### Python

```python
# server.py
from mcp.server.fastmcp import FastMCP

# Create an MCP server
mcp = FastMCP("Demo")
```

### .NET

```csharp
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;
using Microsoft.Extensions.Logging;
using ModelContextProtocol.Server;
using System.ComponentModel;

var builder = Host.CreateApplicationBuilder(args);
builder.Logging.AddConsole(consoleLogOptions =>
{
    // Configure all logs to go to stderr
    consoleLogOptions.LogToStandardErrorThreshold = LogLevel.Trace;
});

builder.Services
    .AddMcpServer()
    .WithStdioServerTransport()
    .WithToolsFromAssembly();
await builder.Build().RunAsync();

// add features
```

### Java

Java को लागि, मुख्य सर्भर कम्पोनेन्टहरू सिर्जना गर्नुहोस्। पहिले मुख्य अनुप्रयोग कक्षा संशोधन गर्नुहोस्:

*src/main/java/com/microsoft/mcp/sample/server/McpServerApplication.java*:

```java
package com.microsoft.mcp.sample.server;

import org.springframework.ai.tool.ToolCallbackProvider;
import org.springframework.ai.tool.method.MethodToolCallbackProvider;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;
import com.microsoft.mcp.sample.server.service.CalculatorService;

@SpringBootApplication
public class McpServerApplication {

    public static void main(String[] args) {
        SpringApplication.run(McpServerApplication.class, args);
    }
    
    @Bean
    public ToolCallbackProvider calculatorTools(CalculatorService calculator) {
        return MethodToolCallbackProvider.builder().toolObjects(calculator).build();
    }
}
```

क्याल्कुलेटर सेवा सिर्जना गर्नुहोस् *src/main/java/com/microsoft/mcp/sample/server/service/CalculatorService.java*:

```java
package com.microsoft.mcp.sample.server.service;

import org.springframework.ai.tool.annotation.Tool;
import org.springframework.stereotype.Service;

/**
 * Service for basic calculator operations.
 * This service provides simple calculator functionality through MCP.
 */
@Service
public class CalculatorService {

    /**
     * Add two numbers
     * @param a The first number
     * @param b The second number
     * @return The sum of the two numbers
     */
    @Tool(description = "Add two numbers together")
    public String add(double a, double b) {
        double result = a + b;
        return formatResult(a, "+", b, result);
    }

    /**
     * Subtract one number from another
     * @param a The number to subtract from
     * @param b The number to subtract
     * @return The result of the subtraction
     */
    @Tool(description = "Subtract the second number from the first number")
    public String subtract(double a, double b) {
        double result = a - b;
        return formatResult(a, "-", b, result);
    }

    /**
     * Multiply two numbers
     * @param a The first number
     * @param b The second number
     * @return The product of the two numbers
     */
    @Tool(description = "Multiply two numbers together")
    public String multiply(double a, double b) {
        double result = a * b;
        return formatResult(a, "*", b, result);
    }

    /**
     * Divide one number by another
     * @param a The numerator
     * @param b The denominator
     * @return The result of the division
     */
    @Tool(description = "Divide the first number by the second number")
    public String divide(double a, double b) {
        if (b == 0) {
            return "Error: Cannot divide by zero";
        }
        double result = a / b;
        return formatResult(a, "/", b, result);
    }

    /**
     * Calculate the power of a number
     * @param base The base number
     * @param exponent The exponent
     * @return The result of raising the base to the exponent
     */
    @Tool(description = "Calculate the power of a number (base raised to an exponent)")
    public String power(double base, double exponent) {
        double result = Math.pow(base, exponent);
        return formatResult(base, "^", exponent, result);
    }

    /**
     * Calculate the square root of a number
     * @param number The number to find the square root of
     * @return The square root of the number
     */
    @Tool(description = "Calculate the square root of a number")
    public String squareRoot(double number) {
        if (number < 0) {
            return "Error: Cannot calculate square root of a negative number";
        }
        double result = Math.sqrt(number);
        return String.format("√%.2f = %.2f", number, result);
    }

    /**
     * Calculate the modulus (remainder) of division
     * @param a The dividend
     * @param b The divisor
     * @return The remainder of the division
     */
    @Tool(description = "Calculate the remainder when one number is divided by another")
    public String modulus(double a, double b) {
        if (b == 0) {
            return "Error: Cannot divide by zero";
        }
        double result = a % b;
        return formatResult(a, "%", b, result);
    }

    /**
     * Calculate the absolute value of a number
     * @param number The number to find the absolute value of
     * @return The absolute value of the number
     */
    @Tool(description = "Calculate the absolute value of a number")
    public String absolute(double number) {
        double result = Math.abs(number);
        return String.format("|%.2f| = %.2f", number, result);
    }

    /**
     * Get help about available calculator operations
     * @return Information about available operations
     */
    @Tool(description = "Get help about available calculator operations")
    public String help() {
        return "Basic Calculator MCP Service\n\n" +
               "Available operations:\n" +
               "1. add(a, b) - Adds two numbers\n" +
               "2. subtract(a, b) - Subtracts the second number from the first\n" +
               "3. multiply(a, b) - Multiplies two numbers\n" +
               "4. divide(a, b) - Divides the first number by the second\n" +
               "5. power(base, exponent) - Raises a number to a power\n" +
               "6. squareRoot(number) - Calculates the square root\n" + 
               "7. modulus(a, b) - Calculates the remainder of division\n" +
               "8. absolute(number) - Calculates the absolute value\n\n" +
               "Example usage: add(5, 3) will return 5 + 3 = 8";
    }

    /**
     * Format the result of a calculation
     */
    private String formatResult(double a, String operator, double b, double result) {
        return String.format("%.2f %s %.2f = %.2f", a, operator, b, result);
    }
}
```

**उत्पादन-तयार सेवाका लागि वैकल्पिक कम्पोनेन्टहरू:**

स्टार्टअप कन्फिगरेसन सिर्जना गर्नुहोस् *src/main/java/com/microsoft/mcp/sample/server/config/StartupConfig.java*:

```java
package com.microsoft.mcp.sample.server.config;

import org.springframework.boot.CommandLineRunner;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class StartupConfig {
    
    @Bean
    public CommandLineRunner startupInfo() {
        return args -> {
            System.out.println("\n" + "=".repeat(60));
            System.out.println("Calculator MCP Server is starting...");
            System.out.println("SSE endpoint: http://localhost:8080/sse");
            System.out.println("Health check: http://localhost:8080/actuator/health");
            System.out.println("=".repeat(60) + "\n");
        };
    }
}
```

हेल्थ कन्ट्रोलर सिर्जना गर्नुहोस् *src/main/java/com/microsoft/mcp/sample/server/controller/HealthController.java*:

```java
package com.microsoft.mcp.sample.server.controller;

import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
import java.time.LocalDateTime;
import java.util.HashMap;
import java.util.Map;

@RestController
public class HealthController {
    
    @GetMapping("/health")
    public ResponseEntity<Map<String, Object>> healthCheck() {
        Map<String, Object> response = new HashMap<>();
        response.put("status", "UP");
        response.put("timestamp", LocalDateTime.now().toString());
        response.put("service", "Calculator MCP Server");
        return ResponseEntity.ok(response);
    }
}
```

अपवाद ह्यान्डलर सिर्जना गर्नुहोस् *src/main/java/com/microsoft/mcp/sample/server/exception/GlobalExceptionHandler.java*:

```java
package com.microsoft.mcp.sample.server.exception;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RestControllerAdvice;

@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(IllegalArgumentException.class)
    public ResponseEntity<ErrorResponse> handleIllegalArgumentException(IllegalArgumentException ex) {
        ErrorResponse error = new ErrorResponse(
            "Invalid_Input", 
            "Invalid input parameter: " + ex.getMessage());
        return new ResponseEntity<>(error, HttpStatus.BAD_REQUEST);
    }

    public static class ErrorResponse {
        private String code;
        private String message;

        public ErrorResponse(String code, String message) {
            this.code = code;
            this.message = message;
        }

        // Getters
        public String getCode() { return code; }
        public String getMessage() { return message; }
    }
}
```

कस्टम ब्यानर सिर्जना गर्नुहोस् *src/main/resources/banner.txt*:

```text
_____      _            _       _             
 / ____|    | |          | |     | |            
| |     __ _| | ___ _   _| | __ _| |_ ___  _ __ 
| |    / _` | |/ __| | | | |/ _` | __/ _ \| '__|
| |___| (_| | | (__| |_| | | (_| | || (_) | |   
 \_____\__,_|_|\___|\__,_|_|\__,_|\__\___/|_|   
                                                
Calculator MCP Server v1.0
Spring Boot MCP Application
```

</details>

### -5- उपकरण र स्रोत थप्दै

तलको कोड थपेर उपकरण र स्रोत थप्नुहोस्:

### TypeScript

```typescript
  server.tool("add",
  { a: z.number(), b: z.number() },
  async ({ a, b }) => ({
    content: [{ type: "text", text: String(a + b) }]
  })
);

server.resource(
  "greeting",
  new ResourceTemplate("greeting://{name}", { list: undefined }),
  async (uri, { name }) => ({
    contents: [{
      uri: uri.href,
      text: `Hello, ${name}!`
    }]
  })
);
```

तपाईंको उपकरणले `a` र `b` प्यारामिटरहरू लिन्छ र यस्तो प्रतिक्रिया उत्पादन गर्ने फंक्शन चलाउँछ:

```typescript
{
  contents: [{
    type: "text", content: "some content"
  }]
}
```

तपाईंको स्रोत "greeting" स्ट्रिङमार्फत पहुँचयोग्य छ र `name` प्यारामिटर लिन्छ र उपकरण जस्तै प्रतिक्रिया उत्पादन गर्छ:

```typescript
{
  uri: "<href>",
  text: "a text"
}
```

### Python

```python
# Add an addition tool
@mcp.tool()
def add(a: int, b: int) -> int:
    """Add two numbers"""
    return a + b


# Add a dynamic greeting resource
@mcp.resource("greeting://{name}")
def get_greeting(name: str) -> str:
    """Get a personalized greeting"""
    return f"Hello, {name}!"
```

माथिको कोडमा हामीले:

- `add` नामक उपकरण परिभाषित गरेका छौं जसले `a` र `p` नामक पूर्णांक प्यारामिटरहरू लिन्छ।
- `greeting` नामक स्रोत सिर्जना गरेका छौं जसले `name` प्यारामिटर लिन्छ।

### .NET

यसलाई तपाईंको Program.cs फाइलमा थप्नुहोस्:

```csharp
[McpServerToolType]
public static class CalculatorTool
{
    [McpServerTool, Description("Adds two numbers")]
    public static string Add(int a, int b) => $"Sum {a + b}";
}
```

### Java

उपकरणहरू पहिले नै अघिल्लो चरणमा सिर्जना भइसकेका छन्।

### -6- अन्तिम कोड

सर्भर सुरु गर्न आवश्यक अन्तिम कोड थपौं:

### TypeScript

```typescript
// Start receiving messages on stdin and sending messages on stdout
const transport = new StdioServerTransport();
await server.connect(transport);
```

पूर्ण कोड यहाँ छ:

```typescript
// index.ts
import { McpServer, ResourceTemplate } from "@modelcontextprotocol/sdk/server/mcp.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";
import { z } from "zod";

// Create an MCP server
const server = new McpServer({
  name: "Demo",
  version: "1.0.0"
});

// Add an addition tool
server.tool("add",
  { a: z.number(), b: z.number() },
  async ({ a, b }) => ({
    content: [{ type: "text", text: String(a + b) }]
  })
);

// Add a dynamic greeting resource
server.resource(
  "greeting",
  new ResourceTemplate("greeting://{name}", { list: undefined }),
  async (uri, { name }) => ({
    contents: [{
      uri: uri.href,
      text: `Hello, ${name}!`
    }]
  })
);

// Start receiving messages on stdin and sending messages on stdout
const transport = new StdioServerTransport();
await server.connect(transport);
```

### Python

```python
# server.py
from mcp.server.fastmcp import FastMCP

# Create an MCP server
mcp = FastMCP("Demo")


# Add an addition tool
@mcp.tool()
def add(a: int, b: int) -> int:
    """Add two numbers"""
    return a + b


# Add a dynamic greeting resource
@mcp.resource("greeting://{name}")
def get_greeting(name: str) -> str:
    """Get a personalized greeting"""
    return f"Hello, {name}!"

# Main execution block - this is required to run the server
if __name__ == "__main__":
    mcp.run()
```

### .NET

Program.cs फाइल निम्न सामग्रीसहित सिर्जना गर्नुहोस्:

```csharp
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;
using Microsoft.Extensions.Logging;
using ModelContextProtocol.Server;
using System.ComponentModel;

var builder = Host.CreateApplicationBuilder(args);
builder.Logging.AddConsole(consoleLogOptions =>
{
    // Configure all logs to go to stderr
    consoleLogOptions.LogToStandardErrorThreshold = LogLevel.Trace;
});

builder.Services
    .AddMcpServer()
    .WithStdioServerTransport()
    .WithToolsFromAssembly();
await builder.Build().RunAsync();

[McpServerToolType]
public static class CalculatorTool
{
    [McpServerTool, Description("Adds two numbers")]
    public static string Add(int a, int b) => $"Sum {a + b}";
}
```

### Java

तपाईंको मुख्य अनुप्रयोग कक्षा यसरी देखिनु पर्छ:

```java
// McpServerApplication.java
package com.microsoft.mcp.sample.server;

import org.springframework.ai.tool.ToolCallbackProvider;
import org.springframework.ai.tool.method.MethodToolCallbackProvider;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;
import com.microsoft.mcp.sample.server.service.CalculatorService;

@SpringBootApplication
public class McpServerApplication {

    public static void main(String[] args) {
        SpringApplication.run(McpServerApplication.class, args);
    }
    
    @Bean
    public ToolCallbackProvider calculatorTools(CalculatorService calculator) {
        return MethodToolCallbackProvider.builder().toolObjects(calculator).build();
    }
}
```

### -7- सर्भर परीक्षण गर्नुहोस्

तलको आदेशले सर्भर सुरु गर्नुहोस्:

### TypeScript

```sh
npm run build
```

### Python

```sh
mcp run server.py
```

> MCP Inspector प्रयोग गर्न, `mcp dev server.py` प्रयोग गर्नुहोस् जसले स्वचालित रूपमा Inspector सुरु गर्छ र आवश्यक प्रोक्सी सेसन टोकन प्रदान गर्छ। यदि `mcp run server.py` प्रयोग गर्दै हुनुहुन्छ भने, तपाईंले म्यानुअली Inspector सुरु गरी कनेक्शन कन्फिगर गर्नुपर्नेछ।

### .NET

पक्का गर्नुहोस् कि तपाईं प्रोजेक्ट डाइरेक्टरीमा हुनुहुन्छ:

```sh
cd McpCalculatorServer
dotnet run
```

### Java

```bash
./mvnw clean install -DskipTests
java -jar target/calculator-server-0.0.1-SNAPSHOT.jar
```

### -8- Inspector प्रयोग गरेर चलाउनुहोस्

Inspector एउटा उत्कृष्ट उपकरण हो जसले तपाईंको सर्भर सुरु गर्छ र तपाईंलाई यससँग अन्तरक्रिया गर्न दिन्छ ताकि तपाईं परीक्षण गर्न सक्नुहोस् कि यो काम गर्छ। सुरु गरौं:

> [!NOTE]
> "command" फिल्डमा फरक देखिन सक्छ किनकि यसमा तपाईंको विशेष रनटाइमसँग सर्भर चलाउने आदेश हुन्छ।

### TypeScript

```sh
npx @modelcontextprotocol/inspector node build/index.js
```

वा यसलाई तपाईंको *package.json* मा यसरी थप्नुहोस्: `"inspector": "npx @modelcontextprotocol/inspector node build/index.js"` र त्यसपछि `npm run inspect` चलाउनुहोस्।

Python ले Node.js उपकरण inspector लाई र्याप गर्छ। यसलाई यसरी कल गर्न सकिन्छ:

```sh
mcp dev server.py
```

तर यसले सबै उपलब्ध मेथडहरू कार्यान्वयन गर्दैन, त्यसैले सिधै Node.js उपकरण चलाउन सिफारिस गरिन्छ:

```sh
npx @modelcontextprotocol/inspector mcp run server.py
```

यदि तपाईंले स्क्रिप्टहरू चलाउन आदेश र आर्गुमेन्टहरू कन्फिगर गर्न सक्ने उपकरण वा IDE प्रयोग गर्दै हुनुहुन्छ भने, `Command` फिल्डमा `python` र `Arguments` मा `server.py` सेट गर्नुहोस्। यसले स्क्रिप्ट सही रूपमा चल्न सुनिश्चित गर्छ।

### .NET

पक्का गर्नुहोस् कि तपाईं प्रोजेक्ट डाइरेक्टरीमा हुनुहुन्छ:

```sh
cd McpCalculatorServer
npx @modelcontextprotocol/inspector dotnet run
```

### Java

क्याल्कुलेटर सर्भर चलिरहेको छ भनेर सुनिश्चित गर्नुहोस्  
Inspector चलाउनुहोस्:

```cmd
npx @modelcontextprotocol/inspector
```

Inspector वेब इन्टरफेसमा:

1. "SSE" लाई ट्रान्सपोर्ट प्रकारको रूपमा चयन गर्नुहोस्
2. URL सेट गर्नुहोस्: `http://localhost:8080/sse`
3. "Connect" क्लिक गर्नुहोस्

![Connect](../../../../translated_images/tool.163d33e3ee307e209ef146d8f85060d2f7e83e9f59b3b1699a77204ae0454ad2.ne.png)

**अब तपाईं सर्भरसँग जडान हुनुहुन्छ**  
**Java सर्भर परीक्षण खण्ड अब पूरा भयो**

अर्को खण्ड सर्भरसँग अन्तरक्रिया गर्ने बारेमा हो।

तपाईंले निम्न प्रयोगकर्ता इन्टरफेस देख्नु पर्नेछ:

![Connect](../../../../translated_images/connect.141db0b2bd05f096fb1dd91273771fd8b2469d6507656c3b0c9df4b3c5473929.ne.png)

1. Connect बटन चयन गरेर सर्भरसँग जडान गर्नुहोस्  
  जडान भएपछि तपाईंले निम्न देख्नु पर्नेछ:

  ![Connected](../../../../translated_images/connected.73d1e042c24075d386cacdd4ee7cd748c16364c277d814e646ff2f7b5eefde85.ne.png)

1. "Tools" र "listTools" चयन गर्नुहोस्, तपाईंले "Add" देख्नु पर्नेछ, "Add" चयन गरी प्यारामिटर मानहरू भर्नुहोस्।

  तपाईंले निम्न प्रतिक्रिया देख्नु पर्नेछ, अर्थात् "add" उपकरणबाट परिणाम:

  ![Result of running add](../../../../translated_images/ran-tool.a5a6ee878c1369ec1e379b81053395252a441799dbf23416c36ddf288faf8249.ne.png)

बधाई छ, तपाईंले आफ्नो पहिलो सर्भर सफलतापूर्वक सिर्जना र चलाउनुभयो!

### आधिकारिक SDK हरू

MCP ले विभिन्न भाषाहरूका लागि आधिकारिक SDK हरू प्रदान गर्छ:

- [C# SDK](https://github.com/modelcontextprotocol/csharp-sdk) - Microsoft सँग सहकार्यमा मर्मत गरिएको
- [Java SDK](https://github.com/modelcontextprotocol/java-sdk) - Spring AI सँग सहकार्यमा मर्मत गरिएको
- [TypeScript SDK](https://github.com/modelcontext
- [Kotlin SDK](https://github.com/modelcontextprotocol/kotlin-sdk) - आधिकारिक Kotlin कार्यान्वयन
- [Swift SDK](https://github.com/modelcontextprotocol/swift-sdk) - Loopwork AI सँग सहकार्यमा मर्मत गरिएको
- [Rust SDK](https://github.com/modelcontextprotocol/rust-sdk) - आधिकारिक Rust कार्यान्वयन

## मुख्य बुँदाहरू

- भाषा-विशिष्ट SDK हरूको साथ MCP विकास वातावरण सेटअप गर्न सजिलो छ
- MCP सर्भरहरू निर्माण गर्दा स्पष्ट स्किमासहित उपकरणहरू सिर्जना र दर्ता गर्नुपर्छ
- भरपर्दो MCP कार्यान्वयनका लागि परीक्षण र डिबगिङ आवश्यक छ

## नमूनाहरू

- [Java Calculator](../samples/java/calculator/README.md)
- [.Net Calculator](../../../../03-GettingStarted/samples/csharp)
- [JavaScript Calculator](../samples/javascript/README.md)
- [TypeScript Calculator](../samples/typescript/README.md)
- [Python Calculator](../../../../03-GettingStarted/samples/python)

## कार्य

आफ्नो रोजाइको उपकरणसहित सरल MCP सर्भर बनाउनुहोस्:

1. आफ्नो मनपर्ने भाषामा उपकरण कार्यान्वयन गर्नुहोस् (.NET, Java, Python, वा JavaScript)।
2. इनपुट प्यारामिटरहरू र रिटर्न मानहरू परिभाषित गर्नुहोस्।
3. सर्भरले अपेक्षित रूपमा काम गर्छ कि गर्दैन भनेर सुनिश्चित गर्न इन्स्पेक्टर उपकरण चलाउनुहोस्।
4. विभिन्न इनपुटहरूसँग कार्यान्वयन परीक्षण गर्नुहोस्।

## समाधान

[Solution](./solution/README.md)

## थप स्रोतहरू

- [Build Agents using Model Context Protocol on Azure](https://learn.microsoft.com/azure/developer/ai/intro-agents-mcp)
- [Remote MCP with Azure Container Apps (Node.js/TypeScript/JavaScript)](https://learn.microsoft.com/samples/azure-samples/mcp-container-ts/mcp-container-ts/)
- [.NET OpenAI MCP Agent](https://learn.microsoft.com/samples/azure-samples/openai-mcp-agent-dotnet/openai-mcp-agent-dotnet/)

## अब के गर्ने

अर्को: [Getting Started with MCP Clients](../02-client/README.md)

**अस्वीकरण**:  
यो दस्तावेज AI अनुवाद सेवा [Co-op Translator](https://github.com/Azure/co-op-translator) प्रयोग गरी अनुवाद गरिएको हो। हामी शुद्धताका लागि प्रयासरत छौं, तर कृपया ध्यान दिनुहोस् कि स्वचालित अनुवादमा त्रुटि वा अशुद्धता हुन सक्छ। मूल दस्तावेज यसको मूल भाषामा नै अधिकारिक स्रोत मानिनुपर्छ। महत्वपूर्ण जानकारीका लागि व्यावसायिक मानव अनुवाद सिफारिस गरिन्छ। यस अनुवादको प्रयोगबाट उत्पन्न कुनै पनि गलतफहमी वा गलत व्याख्याका लागि हामी जिम्मेवार छैनौं।