

#  Groundedness detection API private preview documentation  ![informational](https://shields.io/badge/-PrivatePreview-PrivatePreview) 

The Groundedness detection API detects whether the text responses of large language models (LLMs) are grounded in the source materials provided by the users. Ungroundedness refers to instances where the LLMs produce information that is non-factual or inaccurate from what was present in the source materials.


##  Key Features
- **Domain Selection**: This feature offers the option of predefined domains: either `medical` or `generic`. Users can choose an established domain to ensure more tailored detection that aligns with the specific needs of their field.
- **Task Specification**: This feature lets you select the task you're doing, such as QnA (questioning & answering) and Summarization, with adjustable settings according to the task types.
- **Speed vs Interpretability**: There are two modes that trade off speed with performance.
   - Non-Reasoning mode: Offers fast detection capability, easy to embed into online applications.
   - Reasoning mode: Offers detailed explanation for detected ungrounded segments easy for understanding and mitigation.


This documentation site is structured into the following sections.

- **Concepts** This section offers a deep dive into the fundamental principles and theories underlying the Groundedness detection API. It covers key concepts such as the definition of 'ungroundedness' in the context of AI-generated content.
- **Use cases** provides comprehensive details about the various tasks the API can perform.
- **QuickStart** step-by-step instructions to help users begin making requests to the service.
- **Sample Code** demonstrates practical usage with examples in cURL, Python, C#, and Java.


##  Concepts

- **Retrieval Augmented Generation (RAG)**: RAG is a technique for augmenting LLM knowledge with additional data. LLMs can reason about wide-ranging topics, but their knowledge is limited to the public data up to a specific point in time that they were trained on. If you want to build AI applications that can reason about private data or data introduced after a model’s cutoff date, you need to augment the knowledge of the model with the specific information it needs. The process of bringing the appropriate information and inserting it into the model prompt is known as Retrieval Augmented Generation (RAG). For more information, see [Retrieval-augmented generation (RAG)](https://python.langchain.com/docs/use_cases/question_answering/).

- **Groundedness and Ungroundedness in LLMs**: This refers to the extent to which the model’s outputs are based on provided information or reflect reliable sources accurately. A grounded response adheres closely to the given information, avoiding speculation or fabrication. In groundedness measures, source information is crucial and serves as the grounding source. 

- **Hallucination in LLMs**: Hallucination involves generating text that contains fabricated, false, or misleading information. Hallucination in LLMs is a broader concept that generally includes ungroundedness. It refers to the generation of text that is not only ungrounded, but also fabricated, false, or misleading. Hallucination encompasses a wider range of inaccuracies, including completely made-up information that may not have any basis in the provided data or known facts.
    - While all hallucinated content is ungrounded, not all ungrounded content is a hallucination. Hallucination is a broader term that includes any kind of fabricated or false information, whereas ungroundedness specifically refers to deviations from provided information or known facts.

##  Use cases

The Groundedness detection supports text-based Summarization and QnA tasks to ensure that the generated summaries or answers are accurate and reliable. Here are some examples of each use case:

**Summarization tasks**:
- Medical summarization: In the context of medical news articles, Groundedness detection can be used to ensure that the summary does not contain fabricated or misleading information, guaranteeing that readers obtain accurate and reliable medical information.
- Academic paper summarization: When generating summaries of academic papers or research articles, the function can help ensure that the summarized content accurately represents the key findings and contributions without introducing false claims.
- Legal document summarization: In legal environments, where summarizing lengthy legal documents is common, the function can confirm that the summary does not contain any erroneous statements or omissions that could lead to legal disputes.

**QnA tasks**:
- Customer support chatbots: In customer support, the function can be used to validate the answers provided by AI chatbots, ensuring that customers receive accurate and trustworthy information when they ask questions about products or services.
- Medical QnA: For medical QnA, the function assists in verifying the accuracy of medical answers and advice provided by AI systems to healthcare professionals and patients, reducing the risk of medical errors.
- Educational QnA: In educational settings, the function can be applied to QnA tasks to confirm that answers to academic questions or test prep queries are factually accurate, supporting the learning process.
- Financial and investment queries: For financial and investment-related questions, the function can validate the answers given by AI systems, helping users make informed financial decisions based on accurate information.

## Limitations

**Language availability**
Currently, the Groundedness detection API supports the English language. While our API does not restrict the submission of non-English content, we cannot guarantee the same level of quality and accuracy in the analysis of other language content. We recommend that users submit content primarily in English to ensure the most reliable and accurate results from the API.

**Text length limitations**
Please note that the maximum character limit for the grounding sources is 55K characters, and for the text, it is 7.5K characters for each API call. If your input (either text or grounding sources) exceeds these character limitations per API call, you will encounter an error.

**RPS limitations**

| Pricing Tier | Requests per 10 second (RPS) |
| :----------- | :--------------------------- |
| F0           | 10                           |
| S0           | 10                           |

If you need a higher RPS, please [contact us](mailto:contentsafetysupport@microsoft.com) to request.

## Quickstart 
### Whitelist your subscription ID

1. Submit this form by filling your subscription ID to whitelist this feature to you: [Microsoft Forms](https://forms.office.com/r/38GYZwLC0u).
2. The whitelist will take up to 48 hours to approve. Once you receive confirmation from Microsoft, you can go to the next step.

### Create an Azure Content Safety resource

1. Sign in to the [Azure Portal](https://portal.azure.com/).
2. [Create Content Safety Resource](https://aka.ms/acs-create). Enter a unique name for your resource, select your whitelisted subscription, resource group, region and pricing tier. Currently the private preview Groundedness detection API is available in three regions: **East US2, West US, Sweden Central**. Please create your Content Safety resource in one of these regions.
3. The resource will take a few minutes to deploy. After it does, go to the new resource. In the left pane, under **Resource Management**, select **API Keys and Endpoints**. Copy one of the subscription key values and endpoint to a temporary location for later use.

### Test with a sample request

Now that you have a resource available in Azure for Content Safety and you have a subscription key for that resource, run some tests with the Groundedness detection API.

1. Substitute the `<endpoint>` with your resource endpoint URL (skip the `https://` in the URL).
1. Replace `<your_subscription_key>` with your key.


```json
{
    "Domain": "GENERIC",
    "Task": "QNA",
    "Query": "How much does she currently get paid per hour at the bank?",
    "Text": "12/hour.",
    "GroundingSources": [
        "I'm 21 years old and I need to make a decision about the next two years of my life. Within a week. I currently work for a bank that requires strict sales goals to meet. IF they aren't met three times (three months) you're canned. They pay me 10/hour and it's not unheard of to get a raise in 6ish months. The issue is, **I'm not a salesperson**. That's not my personality. I'm amazing at customer service. I have the most positive customer service \"reports\" done about me in the short time I've worked here. A coworker asked \"do you ask for people to fill these out? You have a ton\". That being said, I have a job opportunity at Chase Bank as a part time teller. What makes this decision so hard is that at my current job, I get 40 hours and Chase could only offer me 20 hours/week. Drive time to my current job is also 21 miles **one way** while Chase is literally 1.8 miles from my house, allowing me to go home for lunch. I do have an apartment and an awesome roommate that I know wont be late on his portion of rent, so paying bills with 20 hours a week isn't the issue. It's the spending money and being broke all the time.\n\nI previously worked at Wal-Mart and took home just about 400 dollars every other week. So I know i can survive on this income. I just don't know whether I should go for Chase as I could definitely see myself having a career there. I'm a math major likely going to become an actuary, so Chase could provide excellent opportunities for me **eventually**.",
       
    ],
    "Reasoning": true,
    "GptResource": {
        "AzureOpenAIEndpoint": "<Your_GPT_Endpoint>",
        "DeploymentName": "<Your_GPT_Deployment>"
    }
}
```


| Name                   | Description                                                  | Type    |
| :--------------------- | :----------------------------------------------------------- | ------- |
| **Domain** | (Optional) `MEDICAL` or `GENERIC`. Default value: `GENERIC`. | Enum  |
| **Task**               | (Optional) Type of task: `QnA`, `Summarization`. Default value: `Summarization`. | Enum |
| **Query**               | (Optional) This parameter is only used when the task type is QnA in which case it's required. Character limit: 7,500. | String  |
| **Text**          | (Required) The text that needs to be checked. Character limit: 7,500. |  String  |
| **GroundingSources**         | (Required) Uses an array of grounding sources to validate AI-generated text. Restrictions on the total amount of grounding sources that can be analyzed in a single request are 55K characters. | String array    |
| **Reasoning**         | (Optional) Specifies whether to use the reasoning feature. The default value is `False`. If `True`, the service uses our default GPT resources to provided an explanation and included the "ungrounded" sentence. Be careful: using reasoning will increase the processing time and may incur extra fees.| Boolean   |
| **GptResource**         | (Optional) If you want to use your own GPT resources instead of our default GPT resources, add this field manually and include the subfield below for the GPT resources used. Currently, **our default GPT resource does not charge fees, but this will change in public preview. If you do not want to use your own GPT resources, remove this field from the input.** | String   |

GPTResource
| Name                   | Description                                                  | Type    |
| :--------------------- | :----------------------------------------------------------- | ------- |
| **azureOpenAIEndpoint** | Endpoint URL for Azure's OpenAI service.  | String |
| **deploymentName** | Name of the specific deployment to use. | String|


The Groundedness detection API provides the option to include _reasoning_ in the API response. If you opt for reasoning, you must either utilize your own GPT resources or use our provided default GPT resources. In this case, the response will include an additional reasoning value. This value details specific instances and explanations for any detected ungroundedness. If you choose not to receive reasoning, the API will classify the submitted content as `true` or `false` and provide a confidence score.

In order to use your own GPT resources, you need to give your Content Safety resource access to the Azure OpenAI resource. Enable system-assigned Managed identity for the Azure AI Content Safety instance and assign the role of Azure OpenAI Contributor/User to the identity:

1. Enable managed identity for the Azure AI Content Safety instance.
   
    <img width="434" alt="image" src="https://github.com/Azure/Azure-AI-Content-Safety-Private-Preview/assets/36343326/71fb578f-8ef1-4417-9cfd-685144fa9afa">

1. Assign the role of Azure OpenAI Contributor/User to the Managed identity. Any roles highlighted below should work.
     ![image](https://github.com/Azure/Azure-AI-Content-Safety-Private-Preview/assets/36343326/0bdab704-2825-4a78-b9b4-56e72aa19718)

     ![image](https://github.com/Azure/Azure-AI-Content-Safety-Private-Preview/assets/36343326/5df9be34-0929-4dfa-8e5a-edfd653d0e02)


   <img width="434" alt="image" src="https://github.com/Azure/Azure-AI-Content-Safety-Private-Preview/assets/36343326/d4c0a214-f716-45f4-9f2a-6ac16da59b2a">


### Output

```json
{
    "ungrounded": true,
    "confidenceScore": 1,
    "ungroundedPercentage": 1,
    "ungroundedDetails": [
        {
            "text": "12/hour.",
            "reason": "\"They pay me 10/hour\". The hypothesis is not factually aligned with the premise. "
        }
    ]
}

```


| Name                | Description                                                  | Type    |
| :------------------ | :----------------------------------------------------------- | ------- |
| **ungrounded**        | Indicates whether the text exhibits ungroundedness.  | Boolean    |
| **confidenceScore** | The confidence value of the _ungrounded_ designation. The score will range from 0 to 1.	 | Float	 |
| **ungroundedPercentage** | Specifies the proportion of the text identified as ungrounded, expressed as a number between 0 and 1, where 0 indicates no ungrounded content and 1 indicates entirely ungrounded content.| Float	 |
| **ungroundedDetails** | Provides insights into ungrounded content with specific examples and percentages.| String |


UngroundedDetails
| Name                | Description                                                  | Type    |
| :------------------ | :----------------------------------------------------------- | ------- |
| **Text**   |  The specific text that is ungrounded.  | String   |
| **Reason** |  Offers explanations for detected ungroundedness. | String  |


##  📝 Sample Code 
### Python

Python sample request:

```Python
import http.client
import json

conn = http.client.HTTPSConnection("<Endpoint>")
payload = json.dumps({
  "Domain": "MEDICAL",
  "Task": "SUMMARIZATION",
  "Text": "The patient has recently lost weight.",
  "GroundingSources": [
    "Doctor: Good morning, how can I assist you today? Patient: Good morning, Doctor. I've been feeling quite unwell lately. I have a constant headache and I feel tired all the time. Doctor: I'm sorry to hear that. When did you start experiencing these symptoms? Patient: About two weeks ago. Doctor: Have you noticed any other symptoms? Like fever, weight loss, or changes in appetite? Patient: I have lost a few pounds recently. But I thought it was due to stress at work. Doctor: It's important to consider all factors. Stress can indeed cause such symptoms but let's rule out other possible causes. I recommend we do some blood tests to get a clearer picture. Patient: That sounds like a good idea, Doctor. I just want to know what's wrong so I can start feeling better. Doctor: I understand your concern. Once we have the test results, we'll discuss the next steps. It could be something minor that's easily treatable, so try not to worry too much in the meantime. Patient: Thank you. Doctor: You're welcome."
  ],
  "Reasoning": True,
  "GptResource": {
    "AzureOpenAIEndpoint": "<Your_GPT_Endpoint>",
    "DeploymentName": "<Your_GPT_Deployment>"
  }
})
headers = {
  'Ocp-Apim-Subscription-Key': '<your_subscription_key>',
  'Content-Type': 'application/json'
}
conn.request("POST", "/contentsafety/text:detectUngroundedness?api-version=2023-10-30-preview", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))

```

### C#

C# sample request:

```csharp
var client = new HttpClient();
var request = new HttpRequestMessage(HttpMethod.Post, "<Endpoint>contentsafety/text:detectUngroundedness?api-version=2023-10-30-preview");
request.Headers.Add("Ocp-Apim-Subscription-Key", "<your_subscription_key>");
var content = new StringContent("{\r\n    \"Domain\": \"GENERIC\",\r\n    \"Task\": \"qna\",\r\n    \"Query\": \"test\",\r\n    \"Text\": \"The sun rises from the west. In most cultures and scientific understanding, the sun rises in the west, traverses the sky throughout the day, and sets in the west. This is a result of the Earth's rotation, which gives the impression of the sun's apparent movement across the sky. However, in some ancient myths, legends, or cultural beliefs, there might exist different interpretations. \",\r\n    \"GroundingSources\": [\r\n        \"The sun rises from the east due to the visual effect caused by the Earth's rotation. The rotation of the Earth creates the illusion of the sun rising from the horizon. In reality, it's because we stand on the Earth's surface, rotating from west to east at a speed of approximately 1670 kilometers per hour, which causes the movement of the sun across the sky. The Earth's rotation leads to the alternation of day and night, and the sunrise from the east is just a part of this cycle\"\r\n    ],\r\n    \"RuntimeOptions\" : {\r\n        \"DetectMode\" : \"test\"\r\n    }\r\n}", null, "application/json");
request.Content = content;
var response = await client.SendAsync(request);
response.EnsureSuccessStatusCode();
Console.WriteLine(await response.Content.ReadAsStringAsync());
```

### Java

Java sample request:. 

```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
MediaType mediaType = MediaType.parse("application/json");
RequestBody body = RequestBody.create(mediaType, "{\r\n    \"Domain\": \"GENERIC\",\r\n    \"Task\": \"qna\",\r\n    \"Query\": \"test\",\r\n    \"Text\": \"The sun rises from the west. In most cultures and scientific understanding, the sun rises in the west, traverses the sky throughout the day, and sets in the west. This is a result of the Earth's rotation, which gives the impression of the sun's apparent movement across the sky. However, in some ancient myths, legends, or cultural beliefs, there might exist different interpretations. \",\r\n    \"GroundingSources\": [\r\n        \"The sun rises from the east due to the visual effect caused by the Earth's rotation. The rotation of the Earth creates the illusion of the sun rising from the horizon. In reality, it's because we stand on the Earth's surface, rotating from west to east at a speed of approximately 1670 kilometers per hour, which causes the movement of the sun across the sky. The Earth's rotation leads to the alternation of day and night, and the sunrise from the east is just a part of this cycle\"\r\n    ],\r\n    \"RuntimeOptions\" : {\r\n        \"DetectMode\" : \"test\"\r\n    }\r\n}");
Request request = new Request.Builder()
  .url("<Endpoint>/contentsafety/text:detectUngroundedness?api-version=2023-10-30-preview")
  .method("POST", body)
  .addHeader("Ocp-Apim-Subscription-Key", "<your_subscription_key>")
  .addHeader("Content-Type", "application/json")
  .build();
Response response = client.newCall(request).execute();
```

## Key reference 

- [Content Safety Doc](https://aka.ms/acs-doc)


## We're here to help!

If you get stuck, [shoot us an email](mailto:rai_hdms@microsoft.com) or use the feedback widget on the upper right of any page.

We're excited you're here! 

