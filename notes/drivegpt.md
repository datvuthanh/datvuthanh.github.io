# DriveGPT: "The ChatGPT for Autonomous Driving Car"

_June 2023_

tl;dr: The ChatGPT for Autonomous Driving Car

# DriveGPT: "The ChatGPT for Autonomous Driving Car"

_June 2023_

tl;dr: The ChatGPT for Autonomous Driving Car

#### Overall impression
- Drive Language Model has been announced by Haomo [1], an autonomous driving startup in China. It is based on OpenAI's GPT and utilizes a customized Decoder Block.
- I think that DriveGPT may have drawn inspiration from the paper Pix2Seq, as the tokenization approach used by Haomo appears to resemble **Pix2Seq** [2], particularly **Pix2Seq V2** [3].
- I would like to highlight that the planning module can be divided into three sub-modules: mission planning, behavior planning, and local planning. Let me provide more details on each:
    - Global Planning: This high-level planning component determines the optimal route to reach the final destination. For example, we often use Google Maps to find the global path.
    - Behavior Planning: This mid-level planning component determines real-time high-level actions needed to handle dynamic obstacles. For instance, actions may involve lane switching, turning, or stopping.
    - Local Planning: This low-level component focuses on scene optimization to achieve behavior planning objectives.
- DriveGPT primarily focuses on behavior planning and local planning.
#### Key ideas

- **Drive Language Tokens**: Represent the perception signal such as Object Detection, Object Size, Lane Coordinates, etc to token vector. ![](../resources/haomo-drive-language.jpeg)
    - For example, the first scene (left image) can be converted to token vector: The front of ego vehicle (white box) has 5 dynamic cars respect to 5 coordinates <!-- $x_{1}$ --> <img style="transform: translateY(0.1em); background: white;" src="../svg/ubfQOOscRH.svg">, <!-- $x_{2}$ --> <img style="transform: translateY(0.1em); background: white;" src="../svg/4kQUHNWig2.svg">, <!-- $x_{3}$ --> <img style="transform: translateY(0.1em); background: white;" src="../svg/uTAmKqt90F.svg">, <!-- $x_{4}$ --> <img style="transform: translateY(0.1em); background: white;" src="../svg/ugqmiBsHRN.svg"> and <!-- $x_{5}$ --> <img style="transform: translateY(0.1em); background: white;" src="../svg/7MJR4t2haK.svg"> respectively. Moreover, the scene has 3 lanes <!-- $y_{1}$ --> <img style="transform: translateY(0.1em); background: white;" src="../svg/vb3YXSpd9C.svg">, <!-- $y_{2}$ --> <img style="transform: translateY(0.1em); background: white;" src="../svg/qToSbevLTk.svg">, <!-- $y_{3}$ --> <img style="transform: translateY(0.1em); background: white;" src="../svg/Qzqi48f88Y.svg"> and the ego location <!-- $z_{1}$ --> <img style="transform: translateY(0.1em); background: white;" src="../svg/mlPJa1Klbr.svg">. The token vector should be [ <!-- $T(x_{1})$ --> <img style="transform: translateY(0.1em); background: white;" src="../svg/Nk8hMU63xX.svg"> <!-- $T(x_{2})$ --> <img style="transform: translateY(0.1em); background: white;" src="../svg/oDjgdZnp08.svg"> <!-- $T(x_{3})$ --> <img style="transform: translateY(0.1em); background: white;" src="../svg/rGFvD4RA5j.svg"> <!-- $T(x_{4})$ --> <img style="transform: translateY(0.1em); background: white;" src="../svg/fm3Y5DmllM.svg"> <!-- $T(y_{1})$ --> <img style="transform: translateY(0.1em); background: white;" src="../svg/NVtfXlVGe0.svg"> <!-- $T(y_{2})$ --> <img style="transform: translateY(0.1em); background: white;" src="../svg/TLr5EOQXNT.svg"> <!-- $T(y_{3})$ --> <img style="transform: translateY(0.1em); background: white;" src="../svg/0vFdoT4bE0.svg"> <!-- $T(z_{1})$ --> <img style="transform: translateY(0.1em); background: white;" src="../svg/bqaVrjkXMH.svg"> ]
- **DriveGPT**: Haomo have completely switched to the Transformer decoder-only architecture. ![](../resources/haomo-drivegpt.jpeg)
- **Human Feedback Loop**: Similar to OpenAI GPT, the human feedback loop is used to reward and rank the generated token future from model. Basically, follow to Haomo they reguarly rank human planning have the highest score in ranking system. ![](../resources/haomo-drivegpt-rw-model.jpeg)

#### My Thoughts

From my perspective, Haomo's approach appears to be highly theoretical and challenging to implement in practical products. Firstly, I am uncertain if they possess hardware capabilities that are truly robust enough to enable real-time inference of DriveGPT (at least at 20 FPS). In fact, even ChatGPT takes a substantial amount of time to generate a comprehensive response. Secondly, while ChatGPT is widely acknowledged as a valuable supportive tool, its answers are not always entirely accurate and occasionally include fabrications. Consequently, it is difficult to trust that the local planning derived from DriveGPT would not pose a danger to human lives in the vehicle. Ensuring safe driving is a paramount concern when addressing the autonomous vehicle problem. Overall, I believe Haomo should thoroughly consider this matter before proceeding with real-world implementation.


#### References

[1] [Haomo 8th AI Day](https://live.yiche.com/live/266120.html)
[2] [Pix2seq: A Language Modeling Framework for Object Detection](https://arxiv.org/abs/2109.10852)
[3] [A Unified Sequence Interface for Vision Tasks](https://arxiv.org/pdf/2206.07669.pdf)

