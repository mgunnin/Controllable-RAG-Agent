# Sophisticated Controllable Agent for Complex RAG Tasks 🧠📚

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/release/python-380/)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com)

An advanced Retrieval-Augmented Generation (RAG) solution designed to tackle complex questions that simple semantic similarity-based retrieval cannot solve. This project showcases a sophisticated deterministic graph acting as the "brain" of a highly controllable autonomous agent capable of answering non-trivial questions from your own data.

![Demo](graphs/demo.gif)

## 🌟 Key Features

- **Sophisticated Deterministic Graph**: Acts as the "brain" of the agent, enabling complex reasoning.
- **Controllable Autonomous Agent**: Capable of answering non-trivial questions from custom datasets.
- **Hallucination Prevention**: Ensures answers are solely based on provided data, avoiding AI hallucinations.
- **Multi-step Reasoning**: Breaks down complex queries into manageable sub-tasks.
- **Adaptive Planning**: Continuously updates its plan based on new information.
- **Performance Evaluation**: Utilizes `Ragas` metrics for comprehensive quality assessment.
- **Flexible LLM Integration**: Easily adaptable to work with various Language Models, not limited to specific providers.

## 🧠 How It Works

1. **PDF Loading and Processing**: Load PDF documents and split them into chapters.
2. **Text Preprocessing**: Clean and preprocess the text for better summarization and encoding.
3. **Summarization**: Generate extensive summaries of each chapter using large language models.
4. **Book Quotes Database Creation**: Create a database for specific questions that will need access to quotes from the book.
5. **Vector Store Encoding**: Encode the book content and chapter summaries into vector stores for efficient retrieval.
6. **Question Processing**:
   - Anonymize the question by replacing named entities with variables.
   - Generate a high-level plan to answer the anonymized question.
   - De-anonymize the plan and break it down into retrievable or answerable tasks.
7. **Task Execution**:
   - For each task, decide whether to retrieve information or answer based on context.
   - If retrieving, fetch relevant information from vector stores and distill it.
   - If answering, generate a response using chain-of-thought reasoning.
8. **Verification and Re-planning**:
   - Verify that generated content is grounded in the original context.
   - Re-plan remaining steps based on new information.
9. **Final Answer Generation**: Produce the final answer using accumulated context and chain-of-thought reasoning.

## 🔗 Graph Flow Explanation

![Solution Schema](graphs/final_graph_schema.jpeg)

The algorithm represented by the graph follows these steps:

1. Start with the user's question.<br>
2. Anonymize the question by replacing named entities with variables and storing the mapping.<br>
3. Generate a high-level plan to answer the anonymized question using a language model.<br>
4. De-anonymize the plan by replacing the variables with the original named entities.<br>
5. Break down the plan into individual tasks that involve either retrieving relevant information or answering a question based on a given context.<br>
6. For each task in the plan:<br>
&emsp;&emsp;a. Decide whether to retrieve information from chunks db, chapter summaries db, book quotes db, or answer a question based on the task and the current context.<br>
&emsp;&emsp;b. If retrieving information:<br>
&emsp;&emsp;&emsp;&emsp;i. Retrieve relevant information from one of the vector stores based on the task.<br>
&emsp;&emsp;&emsp;&emsp;ii. Distill the retrieved information to keep only the relevant content.<br>
&emsp;&emsp;&emsp;&emsp;iii. Verify that the distilled content is grounded in the original context. If not, distill the content again.<br>
&emsp;&emsp;c. If answering a question:<br>
&emsp;&emsp;&emsp;&emsp;i. Answer the question based on the current context using a language model, using chain of thought.<br>
&emsp;&emsp;&emsp;&emsp;ii. Verify that the generated answer is grounded in the context. If not, answer the question again.<br>
&emsp;&emsp;d. After retrieving or answering, re-plan the remaining steps based on the updated context.<br>
&emsp;&emsp;e. Check if the original question can be answered with the current context. If so, proceed to the final answer step. Otherwise, continue with the next task in the plan.<br>
7. Generate the final answer to the original question based on the accumulated context, using chain of thought.<br>
8. Verify that the final answer is grounded in the context. If not, generate the final answer again.<br>
9. Output the final answer to the user.<br>

## 📊 Evaluation

The solution is evaluated using `Ragas` metrics:
- Answer Correctness
- Faithfulness
- Answer Relevancy
- Context Recall
- Answer Similarity

## 🔍 Use Case: Harry Potter Book Analysis

The algorithm was tested using the first Harry Potter book, allowing for monitoring of the model's reliance on retrieved information versus pre-trained knowledge. This choice enables us to verify whether the model is using its pre-trained knowledge or strictly relying on the retrieved information from vector stores.

### Example Question
**Q: How did the protagonist defeat the villain's assistant?**

To solve this question, the following steps are necessary:

1. Identify the protagonist of the plot.
2. Identify the villain.
3. Identify the villain's assistant.
4. Search for confrontations or interactions between the protagonist and the villain.
5. Deduce the reason that led the protagonist to defeat the assistant.

The agent's ability to break down and solve such complex queries demonstrates its sophisticated reasoning capabilities.

## 🚀 Getting Started

### Prerequisites

- Python 3.8+
- API key for your chosen LLM provider

### Installation

1. Clone the repository:
   ```sh
   git clone https://github.com/NirDiamant/Controllable-RAG-Agent.git
   cd Deterministic-RAG-Agent
   ```

2. Install required packages:
   ```sh
   pip install -r requirements.txt
   ```

3. Set up environment variables:
   Create a `.env` file in the root directory with your API key:
   ```
   LLM_API_KEY=your_llm_api_key
   ```

### Usage

1. Explore the step-by-step tutorial: `sophisticated_rag_agent_harry_potter.ipynb`

2. Run real-time agent visualization:
   ```sh
   streamlit run simulate_agent.py
   ```

## 🛠️ Technologies Used

- LangChain
- FAISS Vector Store
- Streamlit (for visualization)
- Ragas (for evaluation)
- Flexible integration with various LLMs (e.g., OpenAI GPT models, Groq, or others of your choice)

## 💡 Heuristics and Techniques

1. Encoding both book content in chunks, chapter summaries generated by LLM, and quotes from the book.<br>
2. Anonymizing the question to create a general plan without biases or pre-trained knowledge of any LLM involved.<br>
3. Breaking down each task from the plan to be executed by custom functions with full control.<br>
4. Distilling retrieved content for better and accurate LLM generations, minimizing hallucinations.<br>
5. Answering a question based on context using a Chain of Thought, which includes both positive and negative examples, to arrive at a well-reasoned answer rather than just a straightforward response.<br>
6. Content verification and hallucination-free verification as suggested in "Self-RAG: Learning to Retrieve, Generate, and Critique through Self-Reflection" - https://arxiv.org/abs/2310.11511.<br>
7. Utilizing an ongoing updated plan made by an LLM to solve complicated questions. Some ideas are derived from "Plan-and-Solve Prompting" - https://arxiv.org/abs/2305.04091 and the "babyagi" project - https://github.com/yoheinakajima/babyagi.<br>
8. Evaluating the model's performance using `Ragas` metrics like answer correctness, faithfulness, relevancy, recall, and similarity to ensure high-quality answers.<br>

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a pull request or open an issue for any suggestions or improvements.

## 📚 Learn More

- [Lecture Video](https://www.youtube.com/watch?v=b4v7tjxQkvg&ab_channel=Machine%26DeepLearningIsrael)
- [Medium Article](https://medium.com/@nirdiamant21/controllable-agent-for-complex-rag-tasks-bf8cb652fbb3)

## 🙏 Acknowledgements

Special thanks to Elad Levi for the valuable advice and ideas.

## 📄 License

This project is licensed under the Apache-2.0 License - see the [LICENSE](LICENSE) file for details.