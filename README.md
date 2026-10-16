# 1 Designing the Agent
## 1.1 PEAS Specification


| Component | Description |
|------------|-------------|
| **P — Performance Measure** | Minimising the total distance travelled from Nairobi to Kisumu, ensure the safety of the trailer, other road users, and cargo, level of compliance to traffic rules, and reach Kisumu successfully. |
| **E — Environment** | Kenya’s highway network, including roads and junctions connecting towns in Nairobi, Central Highlands, Rift Valley, and Lake Victoria Basin; other vehicles, pedestrians, weather, road conditions, traffic regulations, and road closures. |
| **A — Actuators** | Steering system, accelerator, brakes, transmission, indicators, headlights, horn, and navigation display or route-control interface. |
| **S — Sensors** | GPS receiver, cameras, radar, speedometer, odometer, proximity sensors, road-condition sensors, digital maps, and traffic-information feeds. |


## 1.2 Classification Using the Five Definitions of Agent Environments

The classifications below describe the real-world, autonomous trailer. Some classifications would change if the problem were modelled only as a fixed map with known road distances.

| Definition | Classification | Explanation |
|------------|----------------|-------------|
| **Observability** | **Partially Observable** | The trailer cannot know every road condition, hidden hazard, vehicle intention, or traffic change at all times. Its sensors provide incomplete and sometimes noisy information. |
| **Determinism** | **Stochastic** | Other road users, weather, accidents, and unexpected events can affect the outcome of the trailer’s actions. |
| **Task** | **Sequential** | Each action affects subsequent states. The route chosen at one town determines which towns and roads the trailer can reach next. |
| **Environment Changes** | **Dynamic** | Traffic, weather, road conditions, and closures can change while the agent is deciding or travelling. |
| **Number of Agents** | **Multi-Agent** | Other drivers, pedestrians, cyclists, and road authorities also influence the environment. |


# 2. Bidirectional weighted graph between towns to Kisumu

![alt text](<Blank diagram.png>)

## 2.1 Geographical Abstraction
| Region | Selected Towns / Nodes |
|----------|------------------------|
| **Nairobi** | Nairobi |
| **Central Highlands** | Thika, Murang’a, Nyeri, Nanyuki |
| **Rift Valley** | Limuru, Naivasha, Gilgil, Nakuru, Nyahururu, Mai Mahiu, Narok, Bomet |
| **Lake Victoria Basin** | Kericho, Ahero, Kisumu |

Abstraction keeps the major towns, road connectivity, and distance weights while omitting junctions, street-level geometry, and traffic conditions. In the search tree, successive town states are compressed into regional corridor states; this makes the high-level decision between the northern Rift Valley corridor and southern Narok–Bomet corridor explicit.


![alt text](<Blank diagram (1).png>)
Figure: Regional Search Tree

# 3 Optimal Path

## 3.1 Road Weights Used for the Two Candidate Routes

| Northern Corridor Edge | Distance (km) | Southern Corridor Edge | Distance (km) |
|------------------------|---------------|------------------------|---------------|
| Nairobi ↔ Limuru | 32 | Nairobi ↔ Mai Mahiu | 58 |
| Limuru ↔ Naivasha | 67 | Mai Mahiu ↔ Narok | 77 |
| Naivasha ↔ Gilgil | 31 | Narok ↔ Bomet | 96 |
| Gilgil ↔ Nakuru | 35 | Bomet ↔ Kericho | 57 |
| Nakuru ↔ Kericho | 105 | Kericho ↔ Ahero | 72 |
| Kericho ↔ Ahero | 72 | Ahero ↔ Kisumu | 28 |
| Ahero ↔ Kisumu | 28 | — | — |

Northern route: Nairobi → Limuru → Naivasha → Gilgil → Nakuru → Kericho → Ahero → Kisumu
= 32 + 67 + 31 + 35 + 105 + 72 + 28 = 370 km.

Southern route: Nairobi → Mai Mahiu → Narok → Bomet → Kericho → Ahero → Kisumu
= 58 + 77 + 96 + 57 + 72 + 28 = 388 km.

Difference = 388 − 370 = 18 km. 

Under the stated graph and weights, the NORTHERN route has the lower total distance (370 km). A computational shortest-path check on the graph yields the same route and cost.


![alt text](<WhatsApp Image 2026-10-16 at 16.06.27.jpeg>)
Image: Evidence of groupwork
