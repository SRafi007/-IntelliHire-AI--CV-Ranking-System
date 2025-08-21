# CV Ranking System - Project Concept & Technical Documentation

## 📋 Project Overview

The CV Ranking System is an AI-powered application that automatically evaluates and ranks candidate resumes against job descriptions using advanced NLP and machine learning techniques. The system provides intelligent matching, semantic analysis, and explainable scoring to streamline recruitment processes.

### Key Features
- **Batch CV Processing**: Upload multiple CVs for simultaneous ranking
- **Individual CV Scoring**: Single CV evaluation with detailed feedback
- **Semantic Matching**: Advanced AI-powered content analysis
- **Skill Extraction**: Automated identification of technical and soft skills
- **Explainable Results**: Clear reasoning behind rankings and scores

---

## 🎯 Project Concept

### Core Functionality
The system operates in two primary modes:

1. **Batch Ranking Mode**
   - User uploads Job Description (JD) + Multiple CVs
   - System ranks all CVs against the JD
   - Output: Sorted list with scores and explanations

2. **Individual Assessment Mode**
   - User uploads Job Description + Single CV
   - System provides detailed analysis and rating
   - Output: Comprehensive score breakdown with improvement suggestions

### Technical Approach
- **Hybrid AI Pipeline**: Combines rule-based matching with LLM intelligence
- **Multi-factor Scoring**: Semantic similarity, skill matching, experience relevance
- **Vector Database Integration**: Efficient similarity search using Qdrant
- **Quantized LLM**: Optimized Qwen 2.5 7B (4-bit) for resource efficiency

---

## 🔄 Data Flow Architecture

```
┌─────────────────┐
│   USER INPUT    │
│                 │
│ • Job Description (Text)
│ • CV Files (PDF batch/single)
└─────────┬───────┘
          │
          ▼
┌─────────────────┐
│  FILE VALIDATION│
│                 │
│ • Format Check (PDF only)
│ • Size Validation (<10MB)
│ • Content Verification
└─────────┬───────┘
          │
          ▼
┌─────────────────┐
│  PDF PARSING    │
│                 │
│ • Text Extraction (PyMuPDF)
│ • Structure Recognition
│ • Metadata Extraction
└─────────┬───────┘
          │
          ▼
┌─────────────────┐
│ CONTENT ANALYSIS│
│    (Parallel)   │
│                 │
├─ Skill Extraction Agent
├─ Experience Parser
├─ Education Extractor
└─ Section Classifier
└─────────┬───────┘
          │
          ▼
┌─────────────────┐
│ VECTOR EMBEDDING│
│                 │
│ • Text Chunking
│ • Sentence Transformers
│ • Qdrant Storage
└─────────┬───────┘
          │
          ▼
┌─────────────────┐
│ SEMANTIC MATCHING│
│                 │
│ • JD vs CV Similarity
│ • Skill Proximity
│ • Context Understanding
└─────────┬───────┘
          │
          ▼
┌─────────────────┐
│  LLM ANALYSIS   │
│  (Qwen 2.5 7B)  │
│                 │
│ • Contextual Scoring
│ • Gap Analysis
│ • Explanation Generation
└─────────┬───────┘
          │
          ▼
┌─────────────────┐
│  FINAL SCORING  │
│                 │
│ • Weighted Aggregation
│ • Ranking Algorithm
│ • Confidence Calculation
└─────────┬───────┘
          │
          ▼
┌─────────────────┐
│ RESULT DELIVERY │
│                 │
│ • Ranked List/Score
│ • Detailed Breakdown
│ • Visual Analytics
│ • Export Options
└─────────────────┘
```

---

## 🖥️ Streamlit Application Concept

### User Interface Structure

#### **Main Dashboard**
```
┌─────────────────────────────────────────────────────────────┐
│                    CV Ranking System                        │
├─────────────────────────────────────────────────────────────┤
│  [Batch Ranking] [Single Analysis] [Settings] [History]    │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  📄 Job Description Input                                   │
│  ┌─────────────────────────────────────────────────────┐   │
│  │ Enter job description here...                       │   │
│  │                                                     │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  📁 CV Upload Area                                          │
│  ┌─────────────────────────────────────────────────────┐   │
│  │       Drag & Drop PDF files here                   │   │
│  │           or click to browse                        │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│                    [🚀 Process CVs]                        │
└─────────────────────────────────────────────────────────────┘
```

#### **Results Dashboard**
```
┌─────────────────────────────────────────────────────────────┐
│                      Results                                │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  📊 Overview Metrics                                        │
│  ┌─────────┬─────────┬─────────┬─────────┐                │
│  │Total CVs│Top Score│Avg Score│Time     │                │
│  │   15    │  92%    │  67%    │ 2m 34s  │                │
│  └─────────┴─────────┴─────────┴─────────┘                │
│                                                             │
│  🏆 Ranking Results                                         │
│  ┌─────────────────────────────────────────────────────────┤
│  │ Rank │ Candidate      │ Score │ Match │ Skills │ Gap    ││
│  ├──────┼────────────────┼───────┼───────┼────────┼────────┤│
│  │  1   │ john_doe.pdf   │ 92%   │ 95%   │ 18/20  │ React  ││
│  │  2   │ jane_smith.pdf │ 87%   │ 89%   │ 16/20  │ AWS    ││
│  │  3   │ mike_wilson.pdf│ 78%   │ 82%   │ 14/20  │ Docker ││
│  └──────┴────────────────┴───────┴───────┴────────┴────────┘│
│                                                             │
│  📈 Analytics & Visualizations                             │
└─────────────────────────────────────────────────────────────┘
```

### Page Structure

1. **Upload Page** (`pages/upload.py`)
   - File drag-and-drop interface
   - Job description text area
   - Processing mode selection
   - Real-time validation feedback

2. **Results Page** (`pages/results.py`)
   - Interactive ranking table
   - Detailed CV analysis modals
   - Skill matching visualizations
   - Export functionality

3. **Settings Page** (`pages/settings.py`)
   - Model configuration
   - Scoring weight adjustment
   - Performance tuning options
   - Cache management

---

## 📊 Data Flow Summary

### Input Processing Flow
1. **File Upload** → Streamlit file uploader accepts PDF files
2. **Validation** → Check file format, size, and readability
3. **Parsing** → Extract text content using PyMuPDF
4. **Preprocessing** → Clean text, remove artifacts, normalize format

### Analysis Pipeline Flow
1. **Content Extraction** → Identify CV sections (experience, skills, education)
2. **Skill Recognition** → Multi-method skill extraction (NER + rules + LLM)
3. **Vectorization** → Convert text to embeddings using sentence transformers
4. **Storage** → Store vectors in Qdrant for efficient similarity search

### Scoring Flow
1. **Semantic Similarity** (40% weight) → Compare JD and CV embeddings
2. **Skill Matching** (35% weight) → Direct and related skill comparison
3. **Experience Relevance** (25% weight) → Years of experience and domain match
4. **LLM Enhancement** → Contextual analysis and explanation generation

### Output Generation Flow
1. **Score Aggregation** → Weighted combination of all factors
2. **Ranking** → Sort candidates by final score
3. **Explanation** → Generate human-readable reasoning
4. **Visualization** → Create charts and detailed breakdowns

---

## 👥 Developer Handoff Brief

### Project Structure Overview
```
cv_ranking_system/
├── app/                    # Streamlit frontend
├── src/agents/            # AI processing modules
├── src/parsers/           # PDF and content parsing
├── src/matching/          # Similarity algorithms
├── src/models/            # LLM integration
├── tests/                 # Comprehensive test suite
└── config/               # Configuration management
```

### Key Components

#### 1. **Agent Architecture** (`src/agents/`)
- **CVAnalyzerAgent**: Main orchestrator for CV analysis
- **SkillExtractorAgent**: Specialized skill identification
- **ScorerAgent**: Multi-factor scoring logic
- **RankingAgent**: Final ranking and explanation generation

#### 2. **Processing Pipeline** (`src/parsers/`)
- **PDFParser**: Robust PDF text extraction
- **CVParser**: Structured content identification
- **ContentCleaner**: Text normalization and preprocessing

#### 3. **Matching Engine** (`src/matching/`)
- **SemanticMatcher**: Embedding-based similarity
- **SkillMatcher**: Skill-specific comparison logic
- **ExperienceMatcher**: Years and domain relevance

### Development Guidelines

#### **Code Standards**
- **Type Hints**: All functions must include proper type annotations
- **Docstrings**: Google-style docstrings for all classes and methods
- **Error Handling**: Comprehensive exception handling with logging
- **Testing**: Minimum 80% code coverage required

#### **Performance Requirements**
- **Memory Usage**: Max 4GB GPU memory allocation
- **Processing Time**: <10 seconds per CV analysis
- **Batch Size**: Support up to 20 CVs simultaneously
- **Response Time**: <3 seconds for UI interactions

#### **Configuration Management**
```python
# All settings centralized in config/
- app_config.yaml: Application settings
- model_config.yaml: AI model parameters
- logging_config.yaml: Logging configuration
```

#### **Testing Strategy**
- **Unit Tests**: Individual component testing
- **Integration Tests**: End-to-end pipeline validation
- **Performance Tests**: Memory and speed benchmarks
- **UI Tests**: Streamlit interface functionality

### Dependencies & Requirements

#### **Core Technologies**
- **Python 3.9+**: Main programming language
- **Ollama**: Local LLM inference server
- **Qwen 2.5 7B (4-bit)**: Primary language model
- **Qdrant**: Vector database for embeddings
- **Streamlit**: Web application framework

#### **Key Libraries**
```python
# AI/ML Stack
- langchain: LLM orchestration
- sentence-transformers: Text embeddings
- torch: PyTorch for model inference

# Document Processing
- PyMuPDF (fitz): PDF parsing
- spacy: Natural language processing
- pandas: Data manipulation

# Web & Storage
- streamlit: Web interface
- qdrant-client: Vector database client
- pyyaml: Configuration management
```

### Deployment Architecture

#### **Local Development**
```yaml
# docker-compose.yml
services:
  qdrant:
    image: qdrant/qdrant:latest
    ports: ["6333:6333"]
    
  app:
    build: .
    ports: ["8501:8501"]
    depends_on: [qdrant]
```

#### **Production Considerations**
- **Containerization**: Docker deployment ready
- **Scalability**: Horizontal scaling via load balancing
- **Monitoring**: Comprehensive logging and metrics
- **Security**: Input validation and rate limiting

### Getting Started for New Developers

1. **Environment Setup**
   ```bash
   git clone <repository>
   cd cv_ranking_system
   pip install -r requirements-dev.txt
   docker-compose up -d  # Start Qdrant
   ```

2. **Model Installation**
   ```bash
   ollama pull qwen2.5:7b-instruct-q4_K_M
   ```

3. **Development Workflow**
   ```bash
   pytest tests/          # Run test suite
   streamlit run app/main.py  # Start development server
   ```

4. **Key Files to Understand**
   - `src/agents/cv_analyzer_agent.py`: Main processing logic
   - `app/main.py`: Streamlit application entry point
   - `config/app_config.yaml`: Application configuration
   - `tests/integration/test_pipeline.py`: End-to-end testing

This documentation provides a comprehensive understanding of the CV Ranking System architecture, enabling efficient development and maintenance by the team.
---

## **Folder Structure**


```
cv_ranking_system/
├── app/
│   ├── main.py              # Streamlit app
│   ├── pages/
│   │   ├── upload.py        # File upload interface
│   │   ├── results.py       # Results visualization
│   │   └── settings.py      # App configuration UI
├── src/
│   ├── agents/              # AI processing components
│   │   ├── __init__.py
│   │   ├── cv_analyzer_agent.py      # Main CV analysis
│   │   ├── skill_extractor_agent.py  # Skill extraction
│   │   ├── scorer_agent.py          # Scoring logic
│   │   └── ranking_agent.py         # Final ranking
│   ├── parsers/
│   │   ├── __init__.py
│   │   ├── pdf_parser.py    # PDF text extraction
│   │   └── cv_parser.py     # Structured CV parsing
│   ├── matching/
│   │   ├── __init__.py
│   │   ├── semantic_matcher.py
│   │   └── similarity_engine.py
│   ├── models/
│   │   ├── __init__.py
│   │   └── llm_handler.py   # Ollama integration
│   ├── database/
│   │   ├── __init__.py
│   │   └── vector_db.py     # Qdrant operations
│   ├── utils/
│   │   ├── __init__.py
│   │   ├── logger.py        # Logging configuration
│   │   └── helpers.py       # Utility functions
│   └── config/
│       ├── __init__.py
│       └── settings.py      # Application settings
├── tests/
│   ├── __init__.py
|   ......
│   └── conftest.py          # pytest configuration
├── data/
│   └── logs/                # Application logs
├── config/
├── requirements.txt
├── requirements-dev.txt     # Development dependencies
├── setup.py
├── pytest.ini
├── .env.example
└── README.md
```
