

  AIWattch  -  How does it work?
=================

AIWattch is a Chrome extension that estimates and displays carbon emissions from ChatGPT conversations. The focus is on education and awareness about AI energy consumption.

Core Implementation
-------------------

### Token based approach

#### Token Counting

*   Character-based token estimation
*   Real-time conversation tracking
*   Basic approximation (configurable chars/token ratio)

#### Emissions Calculation

Total Energy = (Input\_Tokens × Input\_Factor + Output\_Tokens × Output\_Factor) × PUE  
Total Emissions = Total Energy × Grid\_Factor

#### Configurable Parameters

1.  **Infrastructure**
    *   PUE (datacenter [Power Usage Effectiveness](https://en.wikipedia.org/wiki/Power_usage_effectiveness))
    *   Default: 1.125 - Range 1.0-2.0
2.  **Energy Grid**
    *   Grid Emissions Factor
    *   Default: 383 gCO2e/kWh
    *   Local: [ElectricityMap](https://app.electricitymaps.com/map/72h)
3.  **Token Estimation**
    *   Characters per Token
    *   Default: 4 chars/token - range (3-5)
4.  **Energy Factors**
    *   Input Token Energy (For context processing): Default: 0.001Wh/token
    *   Output Token Energy (For generation processing): Default: 0.01Wh/token

### Time based approach

#### Emission Calculation:

Energy (kWh) = Base Power (W) × Utilization Factor × Computation Time (s) × PUE ÷ 3600000  
Emissions (kgCO2e) = Energy (kWh) × Emissions Factor (gCO2e/kWh) ÷ 1000

#### Time Measurement Process (Computation Time)

1.  Capture three key timestamps:
    *   T1: User submits query
    *   T2: First token appears in response
    *   T3: Last token appears (response complete)
2.  Calculate computation time:
    *   Initial Phase = T2 - T1 (1-network latency factor)
    *   Streaming Phase = T3 - T2 (primarily ongoing computation)
    *   **Computation Time** = (Streaming Phase) + (Initial Phase × (1-network latency factor))

#### Configurable Parameters

*   GPU Base power values (W): default 350 W, [max power output for Nvidia L40S](https://www.nvidia.com/en-us/data-center/l40s/)
    *   GPU Utilization factor (% GPU utilization per query): default 10%
    *   Network latency (% of computation time): default 10%. To be updated based on specific end user location.
    *   PUE (Power Usage Efficiency)
    *   Emissions factor

Important Limitations
---------------------

### Estimation Accuracy

*   Token count is based on character count approximation on web page using DOM method
*   Cannot detect cached responses
*   Simplified token energy estimations based on the [ecologITs calculator](https://huggingface.co/spaces/genai-impact/ecologits-calculator)
*   Time based calculation is based on estimated GPU power consumption and utilization factor. It does not take into account cluster size and distributed computing across a cluster.

### Scope Limitations

*   Does not account for:
    *   Multimodal processing
    *   Local model deployment
    *   Hardware lifecycle embodied carbon emissions
    *   Data center embodied carbon emissions

### Technical Limitations

*   Basic token estimation
*   Conservative energy calculations
*   Limited to ChatGPT web interface
*   Chrome browser only

### Browser Compatibility

*   Chrome version tracking
*   Minimum version requirements
*   Version compatibility logging

### ChatGPT Interface compatibility

*   Version per documentation.

Sources:
--------

Hugging Face LLM perf leaderboard: [https://huggingface.co/spaces/optimum/llm-perf-leaderboard](https://huggingface.co/spaces/optimum/llm-perf-leaderboard)

Technology Evolution Notice
---------------------------

### Rapid Development of LLM Technology

This extension's methodology reflects the state of LLM technology as of late 2024. The field is evolving rapidly in several directions:

#### 1\. Model Architecture Changes

*   Multimodal models becoming standard
*   Reasoning-focused architectures
*   Local and edge deployment
*   Task-specialized models
*   Agent-based systems

#### 2\. Infrastructure Evolution

*   Distributed computing approaches
*   Edge-first deployments
*   Custom AI hardware
*   Novel cooling solutions
*   Hybrid processing strategies

#### 3\. Efficiency Improvements

*   Advanced model compression
*   Intelligent caching
*   Optimized inference
*   Specialized processors
*   Energy-aware routing

### Impact on Emissions Calculation

Our approach may need adaptation for:

*   Multimodal processing
*   Local-edge hybrid systems
*   New efficiency metrics
*   Agent interactions

#### Commitment to Updates

This project will:

*   Track industry developments
*   Update calculation methods
*   Adapt to new architectures
*   Maintain transparency about limitations
*   Incorporate community feedback

#### Version History

Each release will document:

*   Updated assumptions
*   New calculation methods
*   Changed parameters
*   Source updates

We encourage users to check for updates, review methodology changes, contribute insights and share use cases