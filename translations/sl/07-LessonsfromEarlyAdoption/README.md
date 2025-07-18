<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "26d41919cb423a87e067a3da8334e44a",
  "translation_date": "2025-07-14T04:44:15+00:00",
  "source_file": "07-LessonsfromEarlyAdoption/README.md",
  "language_code": "sl"
}
-->
# Lekcije zgodnjih uporabnikov

## Pregled

Ta lekcija raziskuje, kako so zgodnji uporabniki izkoristili Model Context Protocol (MCP) za reševanje resničnih izzivov in spodbujanje inovacij v različnih panogah. S podrobnimi študijami primerov in praktičnimi projekti boste videli, kako MCP omogoča standardizirano, varno in razširljivo integracijo umetne inteligence — povezovanje velikih jezikovnih modelov, orodij in podatkov podjetij v enoten okvir. Pridobili boste praktične izkušnje z načrtovanjem in gradnjo rešitev na osnovi MCP, se naučili preverjenih vzorcev implementacije ter odkrili najboljše prakse za uvajanje MCP v produkcijska okolja. Lekcija prav tako izpostavlja nastajajoče trende, prihodnje smernice in odprtokodne vire, ki vam pomagajo ostati na čelu tehnologije MCP in njenega razvijajočega se ekosistema.

## Cilji učenja

- Analizirati resnične implementacije MCP v različnih panogah  
- Načrtovati in zgraditi celovite aplikacije na osnovi MCP  
- Raziskati nastajajoče trende in prihodnje smeri tehnologije MCP  
- Uporabiti najboljše prakse v dejanskih razvojnih scenarijih  

## Resnične implementacije MCP

### Študija primera 1: Avtomatizacija podpore strankam v podjetju

Mednarodno podjetje je uvedlo rešitev na osnovi MCP za standardizacijo interakcij z umetno inteligenco v svojih sistemih za podporo strankam. To jim je omogočilo:

- Ustvariti enoten vmesnik za več ponudnikov LLM  
- Ohranjati dosledno upravljanje pozivov med oddelki  
- Uvesti robustne varnostne in skladnostne kontrole  
- Enostavno preklapljati med različnimi AI modeli glede na specifične potrebe  

**Tehnična implementacija:**  
```python
# Python MCP server implementation for customer support
import logging
import asyncio
from modelcontextprotocol import create_server, ServerConfig
from modelcontextprotocol.server import MCPServer
from modelcontextprotocol.transports import create_http_transport
from modelcontextprotocol.resources import ResourceDefinition
from modelcontextprotocol.prompts import PromptDefinition
from modelcontextprotocol.tool import ToolDefinition

# Configure logging
logging.basicConfig(level=logging.INFO)

async def main():
    # Create server configuration
    config = ServerConfig(
        name="Enterprise Customer Support Server",
        version="1.0.0",
        description="MCP server for handling customer support inquiries"
    )
    
    # Initialize MCP server
    server = create_server(config)
    
    # Register knowledge base resources
    server.resources.register(
        ResourceDefinition(
            name="customer_kb",
            description="Customer knowledge base documentation"
        ),
        lambda params: get_customer_documentation(params)
    )
    
    # Register prompt templates
    server.prompts.register(
        PromptDefinition(
            name="support_template",
            description="Templates for customer support responses"
        ),
        lambda params: get_support_templates(params)
    )
    
    # Register support tools
    server.tools.register(
        ToolDefinition(
            name="ticketing",
            description="Create and update support tickets"
        ),
        handle_ticketing_operations
    )
    
    # Start server with HTTP transport
    transport = create_http_transport(port=8080)
    await server.run(transport)

if __name__ == "__main__":
    asyncio.run(main())
```

**Rezultati:** 30 % znižanje stroškov modelov, 45 % izboljšanje konsistence odgovorov in izboljšana skladnost v globalnih operacijah.

### Študija primera 2: Diagnostični pomočnik v zdravstvu

Zdravstveni ponudnik je razvil infrastrukturo MCP za integracijo več specializiranih medicinskih AI modelov ob hkratnem zagotavljanju zaščite občutljivih pacientovih podatkov:

- Brezhibno preklapljanje med splošnimi in specialističnimi medicinskimi modeli  
- Stroge kontrole zasebnosti in revizijske sledi  
- Integracija z obstoječimi sistemi elektronskih zdravstvenih kartonov (EHR)  
- Dosledno oblikovanje pozivov za medicinsko terminologijo  

**Tehnična implementacija:**  
```csharp
// C# MCP host application implementation in healthcare application
using Microsoft.Extensions.DependencyInjection;
using ModelContextProtocol.SDK.Client;
using ModelContextProtocol.SDK.Security;
using ModelContextProtocol.SDK.Resources;

public class DiagnosticAssistant
{
    private readonly MCPHostClient _mcpClient;
    private readonly PatientContext _patientContext;
    
    public DiagnosticAssistant(PatientContext patientContext)
    {
        _patientContext = patientContext;
        
        // Configure MCP client with healthcare-specific settings
        var clientOptions = new ClientOptions
        {
            Name = "Healthcare Diagnostic Assistant",
            Version = "1.0.0",
            Security = new SecurityOptions
            {
                Encryption = EncryptionLevel.Medical,
                AuditEnabled = true
            }
        };
        
        _mcpClient = new MCPHostClientBuilder()
            .WithOptions(clientOptions)
            .WithTransport(new HttpTransport("https://healthcare-mcp.example.org"))
            .WithAuthentication(new HIPAACompliantAuthProvider())
            .Build();
    }
    
    public async Task<DiagnosticSuggestion> GetDiagnosticAssistance(
        string symptoms, string patientHistory)
    {
        // Create request with appropriate resources and tool access
        var resourceRequest = new ResourceRequest
        {
            Name = "patient_records",
            Parameters = new Dictionary<string, object>
            {
                ["patientId"] = _patientContext.PatientId,
                ["requestingProvider"] = _patientContext.ProviderId
            }
        };
        
        // Request diagnostic assistance using appropriate prompt
        var response = await _mcpClient.SendPromptRequestAsync(
            promptName: "diagnostic_assistance",
            parameters: new Dictionary<string, object>
            {
                ["symptoms"] = symptoms,
                patientHistory = patientHistory,
                relevantGuidelines = _patientContext.GetRelevantGuidelines()
            });
            
        return DiagnosticSuggestion.FromMCPResponse(response);
    }
}
```

**Rezultati:** Izboljšani diagnostični predlogi za zdravnike ob popolni skladnosti s HIPAA in znatno zmanjšanje preklapljanja konteksta med sistemi.

### Študija primera 3: Analiza tveganj v finančnih storitvah

Finančna institucija je uvedla MCP za standardizacijo procesov analize tveganj v različnih oddelkih:

- Ustvarili so enoten vmesnik za modele kreditnega tveganja, odkrivanja prevar in investicijskega tveganja  
- Uvedli stroge kontrole dostopa in verzioniranje modelov  
- Zagotovili revizijsko sled vseh AI priporočil  
- Ohranjali dosledno obliko podatkov med različnimi sistemi  

**Tehnična implementacija:**  
```java
// Java MCP server for financial risk assessment
import org.mcp.server.*;
import org.mcp.security.*;

public class FinancialRiskMCPServer {
    public static void main(String[] args) {
        // Create MCP server with financial compliance features
        MCPServer server = new MCPServerBuilder()
            .withModelProviders(
                new ModelProvider("risk-assessment-primary", new AzureOpenAIProvider()),
                new ModelProvider("risk-assessment-audit", new LocalLlamaProvider())
            )
            .withPromptTemplateDirectory("./compliance/templates")
            .withAccessControls(new SOCCompliantAccessControl())
            .withDataEncryption(EncryptionStandard.FINANCIAL_GRADE)
            .withVersionControl(true)
            .withAuditLogging(new DatabaseAuditLogger())
            .build();
            
        server.addRequestValidator(new FinancialDataValidator());
        server.addResponseFilter(new PII_RedactionFilter());
        
        server.start(9000);
        
        System.out.println("Financial Risk MCP Server running on port 9000");
    }
}
```

**Rezultati:** Izboljšana skladnost z regulativami, 40 % hitrejši cikli uvajanja modelov in izboljšana konsistentnost ocenjevanja tveganj med oddelki.

### Študija primera 4: Microsoft Playwright MCP strežnik za avtomatizacijo brskalnika

Microsoft je razvil [Playwright MCP strežnik](https://github.com/microsoft/playwright-mcp), ki omogoča varno, standardizirano avtomatizacijo brskalnika preko Model Context Protocol. Ta rešitev omogoča AI agentom in LLM-jem interakcijo z brskalniki na nadzorovan, revizijski in razširljiv način — omogoča primere uporabe, kot so avtomatizirano testiranje spletnih strani, ekstrakcija podatkov in celoviti delovni tokovi.

- Izpostavlja zmogljivosti avtomatizacije brskalnika (navigacija, izpolnjevanje obrazcev, zajem zaslonskih slik itd.) kot MCP orodja  
- Uvaja stroge kontrole dostopa in peskovnik za preprečevanje nepooblaščenih dejanj  
- Ponuja podrobne revizijske zapise vseh interakcij z brskalnikom  
- Podpira integracijo z Azure OpenAI in drugimi ponudniki LLM za avtomatizacijo, ki jo vodijo agenti  

**Tehnična implementacija:**  
```typescript
// TypeScript: Registering Playwright browser automation tools in an MCP server
import { createServer, ToolDefinition } from 'modelcontextprotocol';
import { launch } from 'playwright';

const server = createServer({
  name: 'Playwright MCP Server',
  version: '1.0.0',
  description: 'MCP server for browser automation using Playwright'
});

// Register a tool for navigating to a URL and capturing a screenshot
server.tools.register(
  new ToolDefinition({
    name: 'navigate_and_screenshot',
    description: 'Navigate to a URL and capture a screenshot',
    parameters: {
      url: { type: 'string', description: 'The URL to visit' }
    }
  }),
  async ({ url }) => {
    const browser = await launch();
    const page = await browser.newPage();
    await page.goto(url);
    const screenshot = await page.screenshot();
    await browser.close();
    return { screenshot };
  }
);

// Start the MCP server
server.listen(8080);
```

**Rezultati:**  
- Omogočena varna, programska avtomatizacija brskalnika za AI agente in LLM-je  
- Zmanjšan ročni napor pri testiranju in izboljšano pokritje testov spletnih aplikacij  
- Zagotovljen ponovno uporaben, razširljiv okvir za integracijo orodij na osnovi brskalnika v podjetniških okoljih  

**Reference:**  
- [Playwright MCP Server GitHub Repository](https://github.com/microsoft/playwright-mcp)  
- [Microsoft AI and Automation Solutions](https://azure.microsoft.com/en-us/products/ai-services/)

### Študija primera 5: Azure MCP – Model Context Protocol kot storitev za podjetja

Azure MCP ([https://aka.ms/azmcp](https://aka.ms/azmcp)) je Microsoftova upravljana, podjetniško usmerjena implementacija Model Context Protocol, zasnovana za zagotavljanje razširljivih, varnih in skladnih MCP strežniških zmogljivosti kot oblačne storitve. Azure MCP omogoča organizacijam hitro uvajanje, upravljanje in integracijo MCP strežnikov z Azure AI, podatkovnimi in varnostnimi storitvami, s čimer zmanjšuje operativne stroške in pospešuje sprejemanje AI.

- Popolnoma upravljano gostovanje MCP strežnika z vgrajenim skaliranjem, nadzorom in varnostjo  
- Nativna integracija z Azure OpenAI, Azure AI Search in drugimi Azure storitvami  
- Podjetniška avtentikacija in avtorizacija preko Microsoft Entra ID  
- Podpora za prilagojena orodja, predloge pozivov in povezovalce virov  
- Skladnost s podjetniškimi varnostnimi in regulativnimi zahtevami  

**Tehnična implementacija:**  
```yaml
# Example: Azure MCP server deployment configuration (YAML)
apiVersion: mcp.microsoft.com/v1
kind: McpServer
metadata:
  name: enterprise-mcp-server
spec:
  modelProviders:
    - name: azure-openai
      type: AzureOpenAI
      endpoint: https://<your-openai-resource>.openai.azure.com/
      apiKeySecret: <your-azure-keyvault-secret>
  tools:
    - name: document_search
      type: AzureAISearch
      endpoint: https://<your-search-resource>.search.windows.net/
      apiKeySecret: <your-azure-keyvault-secret>
  authentication:
    type: EntraID
    tenantId: <your-tenant-id>
  monitoring:
    enabled: true
    logAnalyticsWorkspace: <your-log-analytics-id>
```

**Rezultati:**  
- Zmanjšan čas do vrednosti za podjetniške AI projekte z zagotavljanjem pripravljene, skladne MCP strežniške platforme  
- Poenostavljena integracija LLM-jev, orodij in virov podatkov podjetij  
- Izboljšana varnost, opazovanje in operativna učinkovitost za delovne obremenitve MCP  

**Reference:**  
- [Azure MCP Documentation](https://aka.ms/azmcp)  
- [Azure AI Services](https://azure.microsoft.com/en-us/products/ai-services/)

## Študija primera 6: NLWeb  
MCP (Model Context Protocol) je nastajajoči protokol za klepetalne bote in AI asistente za interakcijo z orodji. Vsak primer NLWeb je tudi MCP strežnik, ki podpira eno osnovno metodo, ask, s katero lahko v naravnem jeziku zastavite vprašanje spletni strani. Vrnjeni odgovor uporablja schema.org, široko uporabljeno slovarno oznako za opis spletnih podatkov. Poenostavljeno rečeno, MCP je za NLWeb to, kar je Http za HTML. NLWeb združuje protokole, formate Schema.org in vzorčno kodo, da spletnim mestom omogoči hitro ustvarjanje teh končnih točk, kar koristi tako ljudem preko pogovornih vmesnikov kot strojem preko naravne interakcije agent-agent.

NLWeb ima dve ločeni komponenti:  
- Protokol, zelo preprost za začetek, za vmesnik s spletno stranjo v naravnem jeziku in format, ki uporablja json in schema.org za vrnjeni odgovor. Več podrobnosti v dokumentaciji REST API.  
- Enostavna implementacija (1), ki izkorišča obstoječo označbo za strani, ki jih je mogoče predstaviti kot sezname elementov (izdelki, recepti, znamenitosti, ocene itd.). Skupaj z naborom uporabniških vmesnikov lahko strani enostavno ponudijo pogovorne vmesnike za svojo vsebino. Več o tem v dokumentaciji Life of a chat query.  

**Reference:**  
- [Azure MCP Documentation](https://aka.ms/azmcp)  
- [NLWeb](https://github.com/microsoft/NlWeb)

### Študija primera 7: MCP za Foundry – Integracija Azure AI agentov

Azure AI Foundry MCP strežniki prikazujejo, kako se MCP lahko uporablja za orkestracijo in upravljanje AI agentov ter delovnih tokov v podjetniških okoljih. Z integracijo MCP z Azure AI Foundry lahko organizacije standardizirajo interakcije agentov, izkoristijo upravljanje delovnih tokov Foundry in zagotovijo varne, razširljive uvedbe. Ta pristop omogoča hitro prototipiranje, robustno spremljanje in nemoteno integracijo z Azure AI storitvami, podpira napredne scenarije, kot so upravljanje znanja in ocenjevanje agentov. Razvijalci imajo na voljo enoten vmesnik za gradnjo, uvajanje in spremljanje agentnih procesov, IT ekipe pa pridobijo izboljšano varnost, skladnost in operativno učinkovitost. Rešitev je idealna za podjetja, ki želijo pospešiti sprejemanje AI in ohraniti nadzor nad kompleksnimi procesi, ki jih vodijo agenti.

**Reference:**  
- [MCP Foundry GitHub Repository](https://github.com/azure-ai-foundry/mcp-foundry)  
- [Integrating Azure AI Agents with MCP (Microsoft Foundry Blog)](https://devblogs.microsoft.com/foundry/integrating-azure-ai-agents-mcp/)

### Študija primera 8: Foundry MCP Playground – Eksperimentiranje in prototipiranje

Foundry MCP Playground ponuja okolje, pripravljeno za uporabo, za eksperimentiranje z MCP strežniki in integracijami Azure AI Foundry. Razvijalci lahko hitro prototipirajo, testirajo in ocenjujejo AI modele ter delovne tokove agentov z viri iz Azure AI Foundry kataloga in laboratorijev. Playground poenostavi nastavitev, ponuja vzorčne projekte in podpira sodelovalni razvoj, kar omogoča enostavno raziskovanje najboljših praks in novih scenarijev z minimalnim naporom. Še posebej je uporaben za ekipe, ki želijo potrditi ideje, deliti eksperimente in pospešiti učenje brez potrebe po kompleksni infrastrukturi. Z znižanjem vstopne ovire playground spodbuja inovacije in prispevke skupnosti v ekosistemu MCP in Azure AI Foundry.

**Reference:**  
- [Foundry MCP Playground GitHub Repository](https://github.com/azure-ai-foundry/foundry-mcp-playground)

### Študija primera 9: Microsoft Docs MCP Server – Učenje in usposabljanje  
Microsoft Docs MCP Server implementira Model Context Protocol (MCP) strežnik, ki AI asistentom omogoča dostop v realnem času do uradne Microsoftove dokumentacije. Izvaja semantično iskanje po uradni tehnični dokumentaciji Microsofta.

**Reference:**  
- [Microsoft Learn Docs MCP Server](https://github.com/MicrosoftDocs/mcp)

## Praktični projekti

### Projekt 1: Zgradite MCP strežnik z več ponudniki

**Cilj:** Ustvariti MCP strežnik, ki lahko usmerja zahteve do več ponudnikov AI modelov glede na določena merila.

**Zahteve:**  
- Podpora za vsaj tri različne ponudnike modelov (npr. OpenAI, Anthropic, lokalni modeli)  
- Implementacija mehanizma usmerjanja na podlagi metapodatkov zahtev  
- Ustvariti sistem konfiguracije za upravljanje poverilnic ponudnikov  
- Dodati predpomnjenje za optimizacijo zmogljivosti in stroškov  
- Zgraditi preprost nadzorni plošči za spremljanje uporabe  

**Koraki implementacije:**  
1. Nastavitev osnovne infrastrukture MCP strežnika  
2. Implementacija adapterjev ponudnikov za vsako AI storitev  
3. Ustvarjanje logike usmerjanja na podlagi atributov zahtev  
4. Dodajanje mehanizmov predpomnjenja za pogoste zahteve  
5. Razvoj nadzorne plošče za spremljanje  
6. Testiranje z različnimi vzorci zahtev  

**Tehnologije:** Izberite med Python (.NET/Java/Python glede na vašo izbiro), Redis za predpomnjenje in preprost spletni okvir za nadzorno ploščo.

### Projekt 2: Podjetniški sistem za upravljanje pozivov

**Cilj:** Razviti sistem na osnovi MCP za upravljanje, verzioniranje in uvajanje predlog pozivov v organizaciji.

**Zahteve:**  
- Ustvariti centralizirano skladišče predlog pozivov  
- Implementirati verzioniranje in odobritvene delovne tokove  
- Zgraditi možnosti testiranja predlog z vzorčnimi vnosi  
- Razviti nadzor dostopa na podlagi vlog  
- Ustvariti API za pridobivanje in uvajanje predlog  

**Koraki implementacije:**  
1. Oblikovanje sheme baze podatkov za shranjevanje predlog  
2. Ustvarjanje osnovnega API-ja za CRUD operacije s predlogami  
3. Implementacija sistema verzioniranja  
4. Izgradnja odobritvenega delovnega toka  
5. Razvoj testnega okvira  
6. Ustvarjanje preprostega spletnega vmesnika za upravljanje  
7. Integracija z MCP strežnikom  

**Tehnologije:** Izbira backend okvira, SQL ali NoSQL baze podatkov in frontend okvira za upravljalski vmesnik.

### Projekt 3: Platforma za generiranje vsebin na osnovi MCP

**Cilj:** Zgraditi platformo za generiranje vsebin, ki uporablja MCP za zagotavljanje doslednih rezultatov pri različnih vrstah vsebin.

**Zahteve:**  
- Podpora za več formatov vsebin (blog objave, družbena omrežja, marketinški teksti)  
- Implementacija generiranja na osnovi predlog z možnostmi prilagajanja  
- Ustvariti sistem za pregled in povratne informacije vsebin  
- Spremljati metrike uspešnosti vsebin  
- Podpora verzioniranju in iteraciji vsebin  

**Koraki implementacije:**  
1. Nastavitev MCP odjemalske infrastrukture  
2. Ustvarjanje predlog za različne vrste vsebin  
3. Izgradnja cevovoda za generiranje vsebin  
4. Implementacija sistema za pregled  
5. Razvoj sistema za spremljanje metrik  
6. Ustvarjanje uporabniškega vmesnika za upravljanje predlog in generiranje vsebin  

**Tehnologije:** Vaš izbrani programski jezik, spletni okvir in sistem baze podatkov.

## Prihodnje smernice za tehnologijo MCP

### Nastajajoči trendi

1. **Večmodalni MCP**  
   - Razširitev MCP za standardizacijo interakcij z modeli za slike, zvok in video  
   - Razvoj zmogljivosti za medmodalno sklepanje  
   - Standardizirani formati pozivov za različne modalitete  

2. **Federirana MCP infrastruktura**  
   - Distribuirana MCP omrežja, ki lahko delijo vire med organizacijami  
   - Standardizirani protokoli za varno deljenje modelov  
   - Tehnike zasebnost ohranjajočega računalništva  

3. **MCP tržnice**  
   - Ekosistemi za deljenje in monetizacijo MCP predlog in vtičnikov  
   - Procesi zagotavljanja kakovosti in certificiranja  
   - Integracija s tržnicami modelov  

4. **MCP za edge računalništvo**  
   - Prilagoditev MCP standardov za naprave z omejenimi viri na robu omrežja  
   - Optimizirani protokoli za okolja z nizko pasovno širino  
   - Specializirane implementacije MCP za IoT ekosisteme  

5. **Regulativni okviri**  
   - Razvoj razširitev MCP za skladnost z regulativami  
   - Standardizirane revizijske sledi in vmesniki za razložljivost  
   - Integracija z nastajajočimi okviri za upravljanje AI  

### MCP rešitve iz Microsofta

Microsoft in Azure sta razvila več odprtokodnih repozitorijev, ki razvijalcem pomagajo implementirati MCP v različnih scenarijih:

#### Microsoft organizacija  
1. [playwright-mcp](https://github.com/microsoft/playwright-mcp) – Playwright MCP strežnik za avtomatizacijo in testiranje brskalnika  
2. [files-mcp-server](https://github.com/microsoft/files-mcp-server) – OneDrive MCP strežnik za lokalno testiranje in prispevke skupnosti  
3. [NLWeb](https://github.com/microsoft/Nl
3. [remote-mcp-functions](https://github.com/Azure-Samples/remote-mcp-functions) - Začetna stran za implementacije Remote MCP Server v Azure Functions z povezavami do repozitorijev za posamezne programske jezike  
4. [remote-mcp-functions-python](https://github.com/Azure-Samples/remote-mcp-functions-python) - Predloga za hiter začetek gradnje in nameščanja prilagojenih oddaljenih MCP strežnikov z uporabo Azure Functions in Pythona  
5. [remote-mcp-functions-dotnet](https://github.com/Azure-Samples/remote-mcp-functions-dotnet) - Predloga za hiter začetek gradnje in nameščanja prilagojenih oddaljenih MCP strežnikov z uporabo Azure Functions in .NET/C#  
6. [remote-mcp-functions-typescript](https://github.com/Azure-Samples/remote-mcp-functions-typescript) - Predloga za hiter začetek gradnje in nameščanja prilagojenih oddaljenih MCP strežnikov z uporabo Azure Functions in TypeScripta  
7. [remote-mcp-apim-functions-python](https://github.com/Azure-Samples/remote-mcp-apim-functions-python) - Azure API Management kot AI Gateway do oddaljenih MCP strežnikov z uporabo Pythona  
8. [AI-Gateway](https://github.com/Azure-Samples/AI-Gateway) - Eksperimenti APIM ❤️ AI, vključno z MCP zmogljivostmi, integracija z Azure OpenAI in AI Foundry  

Ti repozitoriji ponujajo različne implementacije, predloge in vire za delo z Model Context Protocol v različnih programskih jezikih in Azure storitvah. Pokrivajo širok spekter primerov uporabe, od osnovnih implementacij strežnikov do avtentikacije, uvajanja v oblak in scenarijev integracije v podjetjih.

#### MCP Resources Directory

[MCP Resources directory](https://github.com/microsoft/mcp/tree/main/Resources) v uradnem Microsoft MCP repozitoriju ponuja skrbno izbrano zbirko vzorčnih virov, predlog pozivov in definicij orodij za uporabo z MCP strežniki. Ta imenik je zasnovan tako, da razvijalcem omogoči hiter začetek z MCP, saj ponuja ponovno uporabne gradnike in primere najboljših praks za:

- **Predloge pozivov:** Pripravljenih za uporabo predlog pozivov za pogoste AI naloge in scenarije, ki jih lahko prilagodite za svoje implementacije MCP strežnikov.  
- **Definicije orodij:** Primeri shem orodij in metapodatkov za standardizacijo integracije in klicev orodij med različnimi MCP strežniki.  
- **Vzorčni viri:** Primeri definicij virov za povezovanje z viri podatkov, API-ji in zunanjimi storitvami znotraj MCP okvira.  
- **Referenčne implementacije:** Praktični primeri, ki prikazujejo, kako strukturirati in organizirati vire, pozive in orodja v realnih MCP projektih.  

Ti viri pospešujejo razvoj, spodbujajo standardizacijo in pomagajo zagotoviti najboljše prakse pri gradnji in uvajanju rešitev na osnovi MCP.

#### MCP Resources Directory
- [MCP Resources (Sample Prompts, Tools, and Resource Definitions)](https://github.com/microsoft/mcp/tree/main/Resources)

### Raziskovalne priložnosti

- Učinkovite tehnike optimizacije pozivov znotraj MCP okvirov  
- Varnostni modeli za večuporabniške MCP namestitve  
- Merjenje zmogljivosti različnih MCP implementacij  
- Formalne metode preverjanja za MCP strežnike  

## Zaključek

Model Context Protocol (MCP) hitro oblikuje prihodnost standardizirane, varne in interoperabilne integracije AI v različnih panogah. Skozi študije primerov in praktične projekte v tej lekciji ste videli, kako zgodnji uporabniki – vključno z Microsoftom in Azure – izkoriščajo MCP za reševanje resničnih izzivov, pospeševanje uvajanja AI ter zagotavljanje skladnosti, varnosti in razširljivosti. Modularni pristop MCP omogoča organizacijam povezovanje velikih jezikovnih modelov, orodij in podatkov podjetja v enoten, pregledni okvir. Ko se MCP še naprej razvija, bo ključnega pomena aktivno sodelovanje v skupnosti, raziskovanje odprtokodnih virov in uporaba najboljših praks za gradnjo robustnih, na prihodnost pripravljenih AI rešitev.

## Dodatni viri

- [MCP Foundry GitHub Repository](https://github.com/azure-ai-foundry/mcp-foundry)  
- [Foundry MCP Playground](https://github.com/azure-ai-foundry/foundry-mcp-playground)  
- [Integracija Azure AI agentov z MCP (Microsoft Foundry Blog)](https://devblogs.microsoft.com/foundry/integrating-azure-ai-agents-mcp/)  
- [MCP GitHub Repository (Microsoft)](https://github.com/microsoft/mcp)  
- [MCP Resources Directory (Sample Prompts, Tools, and Resource Definitions)](https://github.com/microsoft/mcp/tree/main/Resources)  
- [MCP skupnost in dokumentacija](https://modelcontextprotocol.io/introduction)  
- [Azure MCP dokumentacija](https://aka.ms/azmcp)  
- [Playwright MCP Server GitHub Repository](https://github.com/microsoft/playwright-mcp)  
- [Files MCP Server (OneDrive)](https://github.com/microsoft/files-mcp-server)  
- [Azure-Samples MCP](https://github.com/Azure-Samples/mcp)  
- [MCP Auth Servers (Azure-Samples)](https://github.com/Azure-Samples/mcp-auth-servers)  
- [Remote MCP Functions (Azure-Samples)](https://github.com/Azure-Samples/remote-mcp-functions)  
- [Remote MCP Functions Python (Azure-Samples)](https://github.com/Azure-Samples/remote-mcp-functions-python)  
- [Remote MCP Functions .NET (Azure-Samples)](https://github.com/Azure-Samples/remote-mcp-functions-dotnet)  
- [Remote MCP Functions TypeScript (Azure-Samples)](https://github.com/Azure-Samples/remote-mcp-functions-typescript)  
- [Remote MCP APIM Functions Python (Azure-Samples)](https://github.com/Azure-Samples/remote-mcp-apim-functions-python)  
- [AI-Gateway (Azure-Samples)](https://github.com/Azure-Samples/AI-Gateway)  
- [Microsoft AI in avtomatizacijske rešitve](https://azure.microsoft.com/en-us/products/ai-services/)  

## Vaje

1. Analizirajte eno od študij primerov in predlagajte alternativni pristop k implementaciji.  
2. Izberite eno od idej za projekt in pripravite podrobno tehnično specifikacijo.  
3. Raziskujte panogo, ki ni zajeta v študijah primerov, in opišite, kako bi MCP lahko rešil njene specifične izzive.  
4. Raziskujte eno izmed prihodnjih smeri in oblikujte koncept nove MCP razširitve za njeno podporo.  

Naslednje: [Best Practices](../08-BestPractices/README.md)

**Omejitev odgovornosti**:  
Ta dokument je bil preveden z uporabo AI prevajalske storitve [Co-op Translator](https://github.com/Azure/co-op-translator). Čeprav si prizadevamo za natančnost, vas opozarjamo, da avtomatizirani prevodi lahko vsebujejo napake ali netočnosti. Izvirni dokument v njegovem izvirnem jeziku velja za avtoritativni vir. Za ključne informacije priporočamo strokovni človeški prevod. Za morebitna nesporazume ali napačne interpretacije, ki izhajajo iz uporabe tega prevoda, ne odgovarjamo.