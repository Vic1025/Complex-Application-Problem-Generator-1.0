# Math Word Problem Generator

A systematic framework for generating high-quality math word problems with controllable complexity and logical coherence.

## Overview

This project addresses the challenge of creating large-scale, high-quality mathematical reasoning datasets for training language models. Unlike existing approaches that rely heavily on variants of GSM8K, this framework generates diverse, logically consistent math word problems from fundamental mathematical operations.

**Patent No**: CN20688502A

## Key Features

- **Dependency-Based Generation**: Problems are built on tree-structured variable dependencies, ensuring logical coherence
- **Controllable Complexity**: Adjustable tree depth and width to control problem difficulty
- **High Utilization Rate**: ~70-75% of generated problems are valid and usable
- **Diverse Scenarios**: 500+ unique problem themes covering real-world contexts
- **Automatic Validation**: Built-in answer verification and error correction pipeline

## Motivation

Existing math problem generation methods (e.g., GSM8K variants) suffer from:
- High scenario repetition rates (~70% concentrated in 3 story types)
- Disconnected solution steps with meaningless intermediate calculations
- Unrealistic conditions (e.g., fractional people, impossible quantities)

This framework overcomes these limitations through structured dependency modeling.

## Architecture

### Version 1.0 Pipeline

```
1. Random Variable Generation → 
2. Dependency Relationship Construction → 
3. Mathematical Operation Assignment → 
4. Theme Selection → 
5. Background Story & Entity Definition → 
6. Problem Generation → 
7. Answer Validation & Correction
```

![Pipeline Flow](./images/question_generate_process1.5.jpg)

#### Core Components

**1. Variable Dependency Tree**
- Tree-structured relationships (no cycles)
- Example: `A depends on B, C, D` → `B depends on E` → `D depends on F, G`
- Ensures optimal solution paths

![Pipeline Flow](./images/dependency_tree.jpg)

**2. Entity Library (V1.5)**
- Pre-built entity databases organized by themes
- Multiple questioning perspectives for each entity set
- Example: For "Snacks" theme, query price, demand, calories, popularity, etc.

**3. Answer Generation & Validation**
- Automated problem solvability classification
- Cross-verification with model-generated solutions
- Hierarchical dependency mapping for complex problems
- ~60% problems have correct answers on first generation
- ~40% of error cases successfully corrected through re-solving

![Pipeline Flow](./images/answer_generate_process.jpg)

## Data Statistics

Sample datasets with varying complexity:

| Dataset | Max Depth | Max Width | Correct/Error/Unsolvable/Total | 
|---------|-----------|-----------|--------------------------------|
| 2w_10_3_7 | 7 | 3 | 3002/1550/402/5000 |
| 2w_10_3_9 | ≤9 | ≤3 | 1355/409/135/2000 |
| 2w_10_9_9 | ≤9 | ≤9 | 3650/1044/312/5000 |
| 2w5_11_3_9 | ≤9 | ≤3 | 30294/11774/6742/50000 |

## Example Problem

**Generated Problem:**

> At Sunshine Elementary School's Graduation Day, the administration needs to calculate certificates needed. The school has 20 total students, 26 enrolled in the ceremony, 5 transferred, and 1 failed requirements. Students who completed all courses = 40 + dropouts. Students with honors = half of total students. Dropouts = enrolled - failed students. Eligible students = completed all requirements - transferred. Total requirements = completed courses + honors. Certificates needed = eligible students + 50.

**Solution Path:**
```
1. Dropouts = 26 - 1 = 25
2. Honors = 20 / 2 = 10
3. Completed courses = 25 + 40 = 65
4. Total completed = 65 + 10 = 75
5. Eligible = 75 - 5 = 70
6. Certificates = 70 + 50 = 120
```

## Training Results

Model performance on generated datasets:

| Model | Training Data | Test Set | Accuracy |
|-------|---------------|----------|----------|
| tianji-2b-v9-dpo (baseline) | None | op10 | 35.49% |
| tianji-2b-v9-base | op10+op20 (107K) | op10 | 80.68% |
| tianji-2b-v9-base | op10+op20 (107K) | op20 | 69.67% |
| tianji-2b-v9-base | op10+op20 (107K) | op10+op20 | 79.19% |
| qwen2.5-7b-math | - | - | 70.00% |

## Limitations & Future Work

**Current Limitations:**
- Entity definition quality degrades with >10 variables
- Difficulty handling complex subtraction/division dependencies
- Model struggles with highly complex dependency chains

**V2.0 Roadmap:**
- Enhanced entity library with hierarchical structural relationships
- Natural language logic constraints (e.g., implicit distance = speed × time)
- Elimination of redundant common-sense calculation steps
- Improved coherence for multi-level entity relationships

## References

- **Physics 2.1**: [arXiv:2407.20311](https://arxiv.org/pdf/2407.20311)
- **ControlMath**: [arXiv:2409.15376](https://arxiv.org/abs/2409.15376)

## License


