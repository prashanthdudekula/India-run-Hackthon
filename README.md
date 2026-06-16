# India Runs Data & AI Challenge - Candidate Ranking System

**SemanticRank AI** - Intelligent Candidate Discovery Ranker for the Redrob AI Engineer (Founding Team) track

## Overview

This solution ranks 100,000 candidates in under 15 seconds using a highly optimized weighted keyword extraction algorithm combined with behavioral signal multipliers. It meets all Stage 3 Hackathon constraints:
- ⚡ **5-minute execution limit** (completes in ~15 seconds)
- 🖥️ **CPU-only processing** (no GPU required)
- 🚫 **No network calls** (fully offline)

## Features

### Intelligent Scoring Pipeline
- **Semantic Keyword Matching**: Extracts and weights keywords from job descriptions
- **Trap Detection**: Identifies fraudulent profiles (impossible timelines, inconsistent experience)
- **Domain Expertise Validation**: Verifies vector database, ML infrastructure, and retrieval architecture knowledge
- **Behavioral Signals**: Incorporates platform engagement metrics (recruiter response rate, interview completion, activity recency)
- **YOE Optimization**: Prioritizes candidates in the 5-8 year experience sweet spot

### Quality Filters
1. **Honeypot Detection** - Catches mathematically impossible profiles
2. **Product vs. Services Trap** - Filters out pure IT services backgrounds
3. **Pure Research Trap** - Eliminates candidates without production deployments
4. **Vision/Speech vs. NLP/IR Trap** - Ensures NLP/Retrieval focus
5. **LangChain Foundations Trap** - Validates ML infrastructure knowledge

## Installation

### Requirements
- Python 3.7+
- No external dependencies (uses only standard library)

### Setup
```bash
# Navigate to project directory
cd "d:\[PUB] India_runs_data_and_ai_challenge\India_runs_data_and_ai_challenge"

# Run the ranker
python rank.py --candidates candidates.jsonl --out submission.csv
```

## Usage

### Running the Ranker
```bash
python rank.py --candidates candidates.jsonl --out submission.csv
```

**Arguments:**
- `--candidates` (required): Path to candidates file (supports `.jsonl` and `.jsonl.gz`)
- `--out` (required): Output path for submission CSV

**Example:**
```bash
python rank.py --candidates candidates.jsonl --out submission.csv
```

### Validating Submission
```bash
python validate_submission.py submission.csv
```

**Checks:**
- ✅ CSV format validation
- ✅ Header structure (candidate_id, rank, score, reasoning)
- ✅ Exactly 100 data rows
- ✅ Valid candidate ID format (CAND_XXXXXXX)

## Output Format

The submission CSV contains:
- **candidate_id**: Unique identifier (CAND_XXXXXXX)
- **rank**: Position 1-100
- **score**: Weighted composite score (higher = better match)
- **reasoning**: Human-readable explanation for the ranking decision

Example:
```csv
candidate_id,rank,score,reasoning
CAND_0011687,1,269.1,"Matches 'shipper' profile: Hits the sweet spot with 7.8 YOE, strong vector DB experience (weaviate, pinecone), proven background in retrieval/evaluation architecture. Highly engaged on the platform."
CAND_0044883,2,247.25,"Matches 'shipper' profile: Hits the sweet spot with 6.3 YOE, strong vector DB experience (pinecone, faiss), proven background in retrieval/evaluation architecture. Highly engaged on the platform."
```

## Algorithm Details

### Scoring Components
1. **Years of Experience (YOE) Bonus** (+80 points)
   - Sweet spot: 5-8 years
   - Progressive scaling outside this band
   
2. **Vector Database Expertise** (+40 points)
   - Pinecone, Weaviate, Qdrant, Milvus, OpenSearch, Elasticsearch, FAISS
   
3. **Retrieval & Ranking Architecture** (+60 points)
   - Embeddings, hybrid search, BM25, learning-to-rank, evaluation metrics (NDCG, MRR, MAP)
   
4. **ML Infrastructure** (+30 points)
   - PyTorch, TensorFlow, Scikit-learn, XGBoost, LightGBM, LoRA, QLoRA, PEFT
   
5. **Behavioral Multiplier** (0.2x to 1.15x)
   - Recruiter response rate
   - Interview completion rate
   - Recent platform activity
   
6. **Product Company Background** (+20 points)
   - Non-IT services background validation

### Filtering Strategy
Candidates are **disqualified** if they:
- ❌ Have impossible experience timelines (honeypot)
- ❌ Come exclusively from IT services companies
- ❌ Have pure research background without production experience
- ❌ Have 2+ vision/speech skills without NLP/retrieval exposure
- ❌ Use frameworks (LangChain) without ML infrastructure knowledge

## Performance

- **Processing Speed**: ~15 seconds for 100,000 candidates
- **Accuracy**: Top 100 candidates identified with explainable reasoning
- **Memory Usage**: Minimal (all processing in-memory)
- **Output Size**: CSV with 100 candidates (~10 KB)

## Project Structure

```
.
├── rank.py                          # Main ranking algorithm
├── validate_submission.py           # Submission validator
├── submission.csv                   # Output: Top 100 ranked candidates
├── candidate_schema.json            # Candidate data structure definition
├── sample_candidates.json           # Example candidates for testing
├── sample_submission.csv            # Sample output format
├── submission_metadata_template.yaml # Metadata template
├── README.md                        # This file
└── .gitignore                       # Git ignore configuration
```

## Example Workflow

```bash
# 1. Navigate to project
cd "d:\[PUB] India_runs_data_and_ai_challenge\India_runs_data_and_ai_challenge"

# 2. Run the ranker
python rank.py --candidates candidates.jsonl --out submission.csv
# Output: "Successfully wrote top 100 ranked candidates to submission.csv."

# 3. Validate the output
python validate_submission.py submission.csv
# Output: "Submission is valid."

# 4. Review results
# Open submission.csv in Excel or any text editor
```

## Key Design Decisions

1. **Explainability First**: Every ranking includes reasoning to justify the score
2. **Trap-Focused Filtering**: 5 specific disqualification rules based on JD
3. **Behavioral Signals**: Incorporates platform engagement beyond resume content
4. **Performance Optimization**: Uses optimized keyword matching for 100K candidates in 15 seconds
5. **No ML Dependencies**: Pure Python implementation for maximum portability

## Challenge Constraints Met

✅ **5-minute execution limit** - Completes in ~15 seconds  
✅ **CPU-only processing** - No GPU required  
✅ **No network calls** - Fully offline  
✅ **Explainable outputs** - Reasoning provided for each ranking  
✅ **Valid CSV format** - Validated by submission validator  

## Troubleshooting

### "No such file or directory" error
Ensure you're running commands from the correct directory:
```bash
Set-Location -LiteralPath 'd:\[PUB] India_runs_data_and_ai_challenge\India_runs_data_and_ai_challenge'
python rank.py --candidates candidates.jsonl --out submission.csv
```

### Submission validation fails
Check that:
1. File extension is `.csv`
2. Header row is exactly: `candidate_id,rank,score,reasoning`
3. There are exactly 100 data rows (rows 2-101)
4. Candidate IDs match pattern `CAND_XXXXXXX`

### Processing takes too long
- Ensure you're using `.jsonl.gz` (compressed format) for large files
- Check system resources are available
- Verify candidates.jsonl path is correct

## Author & License

**Challenge**: India Runs Data & AI Challenge  
**Track**: Redrob AI Engineer (Founding Team)  
**Date**: June 2026

---

**Build something real. Make hiring smarter.**
