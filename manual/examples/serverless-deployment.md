# Serverless Deployment Example: NeuraCraft in AWS Lambda

Deploying NeuraCraft pipelines in serverless environments enables event-driven AI automation with zero infrastructure management. This example uses AWS Lambda – adapt the principles to Azure Functions or Google Cloud Functions by modifying runtime configurations.

## What We'll Build  
A weather sentiment analysis pipeline that:  
1. Triggers via API Gateway request  
2. Fetches real-time weather data (OpenWeatherMap API)  
3. Runs sentiment analysis on weather description  
4. Returns JSON response with weather + sentiment metadata  

## Prerequisites  
- AWS Account with CLI configured (`aws configure`)  
- Node.js 18.x  
- Serverless Framework: `npm install -g serverless`  
- NeuraCraft CLI: `pip install neurcraft`  

## Step 1: Create Pipeline  
`weather_sentiment.ncpipe`  
```python
from neurcraft.pipes.core import Pipe
from neurcraft.plugins.web import HTTPRequest
from neurcraft.plugins.llm import FastSentiment

class WeatherSentiment(Pipe):
    def workflow(self):
        # Receive location from API trigger
        location = self.trigger.payload['query']['location']
        
        # Get weather data
        weather = HTTPRequest(
            url=f"https://api.openweathermap.org/data/2.5/weather?q={location}",
            params={"appid": "${env:OWM_API_KEY}"}
        ).execute()
        
        # Analyze description sentiment
        description = weather.data['weather'][0]['description']
        sentiment = FastSentiment().analyze(text=description)
        
        # Compose response
        return {
            "location": location,
            "temperature": weather.data['main']['temp'],
            "sentiment_score": sentiment.score,
            "sentiment_label": sentiment.label
        }
```

## Step 2: Configure Serverless Deployment  
`serverless.yml`  
```yaml
service: neura-weather
frameworkVersion: '3'

provider:
  name: aws
  runtime: python3.9
  environment:
    OWM_API_KEY: ${env:OWM_API_KEY}

functions:
  analyze:
    handler: handler.handler
    timeout: 30
    events:
      - httpApi:
          path: /analyze
          method: GET

package:
  patterns:
    - '!**/*'
    - handler.py
    - weather_sentiment.ncpipe
    - '!node_modules/**'
```

## Step 3: Lambda Handler Adapter  
`handler.py`  
```python
from neurcraft.runtime import PipelineExecutor

# Cold start optimization
executor = PipelineExecutor.prepare("weather_sentiment.ncpipe")

def handler(event, context):
    return executor.execute_http(
        event=event,
        output_format="json",
        timeout=25  # Seconds
    )
```

## Step 4: Deploy  
```bash
# Package dependencies
pip install requests neurcraft -t ./vendor
export PYTHONPATH="./vendor"

# Set OpenWeatherMap API key
export OWM_API_KEY="your_api_key_here"

# Deploy
serverless deploy
```

After deployment, you'll receive an endpoint URL:
```
endpoints:
  GET - https://abc123.execute-api.us-east-1.amazonaws.com/analyze
```

## Test the Pipeline  
```bash
curl "https://abc123.execute-api.us-east-1.amazonaws.com/analyze?location=London"
```

Sample Response:
```json
{
  "location": "London",
  "temperature": 283.45,
  "sentiment_score": 0.82,
  "sentiment_label": "positive"
}
```

## Key Optimization Tips  
1. **Cold Starts**:  
   - Use >= 1024MB memory  
   - Enable [Provisioned Concurrency](https://docs.aws.amazon.com/lambda/latest/dg/provisioned-concurrency.html) for production APIs  

2. **State Management**:  
   - For multi-step pipelines, use S3 or DynamoDB for intermediate storage  

3. **Secret Rotation**:  
   - Replace `${env:OWM_API_KEY}` with AWS Secrets Manager references in production  

## Clean Up  
```bash
serverless remove
```

---

> **Gotcha Watch**:  
> - Ensure all Python dependencies are vendored (include `neurcraft` core)  
> - Set timeout 5 sec less than Lambda maximum to allow graceful shutdown  
> - Use API Gateway payload format 2.0 for cleaner event parsing  

Modify this template for batch processing (S3 triggers), scheduled events (CloudWatch), or streaming (Kinesis) by changing event sources in `serverless.yml`.  