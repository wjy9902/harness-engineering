# Key Mental Models

Use these when explaining harness engineering concepts to the user:

- **Watt's Governor**: before it, humans watched the steam engine and adjusted valves by hand. After it, a mechanical feedback loop did it automatically. Harness engineering is the same shift for code.

- **Kubernetes Analogy**: you declare desired state, the controller reconciles. Harness engineering = declaring what "correct" looks like and letting the system converge.

- **The Compound Effect**: when one engineer encodes their expertise (e.g., React patterns), every agent on the team benefits immediately. Expertise multiplies across the agent fleet.

- **Generation vs Verification**: generating correct code is harder than verifying it (P vs NP). The engineer's irreplaceable role is defining and verifying correctness, not generating code.

- **The Invisible Knowledge Problem**: anything not in the repo doesn't exist for agents. Slack discussions, mental models, "how we do things here" -- all invisible until externalized.

- **The Synchronization Paradox**: as agent velocity increases, human communication must also increase, not decrease. OpenAI found they needed 30-minute daily standups because "it can be weeks before I realize core architectural patterns have changed." Faster agents make human alignment MORE important, not less. When setting up a harness, remind the user of this -- the harness handles code quality, but humans still need to synchronize on direction.
