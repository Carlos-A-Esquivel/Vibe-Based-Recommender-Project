# Resonance

A cross-domain recommendation system that suggests movies, books, shows, and home decor based on what you love and what your space looks like.

> **Status:** Early development. Currently in the data exploration phase of the core recommender. See the [Roadmap](#roadmap) for progress.

## Overview

Most recommenders stay inside one domain: a movie app recommends movies, a store recommends products. This project connects them. Your taste in one area should inform recommendations in another, and that includes the physical space you live in.

The system has two layers that work together:

1. **Core recommender.** A ratings-based and content-based recommender for movies, books, and shows, including cross-domain suggestions (for example, books recommended from your movie ratings).
2. **Vibe layer.** Inspired by how people use Pinterest to plan the look of a room or apartment. Upload a photo of your room or describe what you're going for, and the system analyzes its aesthetic and themes, then recommends matching decor *and* entertainment.

## The Vibe Layer

The vibe layer reads two different kinds of signal from a room:

- **Aesthetic** is how a space looks: colors, materials, style, and mood, such as cozy, minimalist, rustic, or industrial.
- **Theme** is what a space is about: the franchises, genres, and interests reflected in its decorations, such as Star Wars, anime, fantasy, or space.

A room can be minimalist in aesthetic and sci-fi in theme. Capturing both is what lets the system make recommendations that feel personal rather than generic.

### Examples

**Room to entertainment.** A room has a Star Wars poster and an anime Funko Pop. The system detects the themes "space / sci-fi" and "anime," and recommends *Cowboy Bebop*, a space-western anime. It isn't simply recommending more of the same franchise; it finds titles where multiple detected themes overlap.

**Entertainment to decor.** A user rates *The Lord of the Rings* highly. The system recommends a carved "Now Serving: Breakfast, Second Breakfast, Elevenses..." kitchen sign referencing hobbit meals, connecting the franchise to decor that references it.

### How It Works (Planned)

- **Shared embedding space.** [CLIP](https://openai.com/research/clip) maps images and text into the same vector space, so a room photo, a style description, and a product image can be compared directly.
- **Zero-shot theme detection.** CLIP scores room images against text labels (franchises, genres, styles) without task-specific training.
- **Theme vocabulary.** Items in every domain are described with a shared set of themes, which acts as the bridge between rooms and entertainment. For movies, the MovieLens tag genome provides a starting point.
- **Franchise linking.** A knowledge graph connects franchises to related movies, books, shows, and merchandise across domains.

## Tech Stack (Planned)

| Area | Tools |
| ---- | ----- |
| Data and ML | Python, pandas, scikit-learn, PyTorch, CLIP |
| Backend | FastAPI, PostgreSQL |
| Frontend | React |
| Infrastructure | Docker, AWS |

## Roadmap

The project is built in layers, so each stage is functional and demo-able on its own.

- [ ] **Phase 1: Core recommender**
  - [ ] Data exploration (in progress)
  - [ ] Evaluation framework and popularity baseline
  - [ ] Collaborative filtering
  - [ ] Content-based recommendations
- [ ] **Phase 2: Cross-domain recommendations** (movies, books, shows)
- [ ] **Phase 3: Backend, frontend, and deployment**
- [ ] **Phase 4: Vibe layer, aesthetic matching** (whole-room style analysis)
- [ ] **Phase 5: Vibe layer, theme detection** (franchise and genre recognition, decor domain)
- [ ] **Stretch goals**
  - [ ] Object detection for individual items
  - [ ] OCR for text-based decor
  - [ ] Blending room photos with style goals
  - [ ] Music as an additional domain, including recognizing music posters and album art

## Evaluation

The core recommender is evaluated offline using a time-based train/test split, which trains on older ratings and tests on newer ones so the model can't "see the future." Results are measured with ranking metrics such as precision@k and NDCG, compared against a popularity baseline.

The vibe layer is harder to measure, because no dataset defines the "correct" recommendations for a room. It will be evaluated qualitatively with a set of test rooms and hand-judged results, along with feedback from real users.

## Early Findings

From exploring the MovieLens dataset (see [`notebooks/01_data_exploration.ipynb`](notebooks/01_data_exploration.ipynb)):

- Ratings lean positive and cluster between 3 and 4 stars, consistent with selection bias: people mostly rate what they chose to watch.
- Activity is highly concentrated. The top 10% of users contribute about 48% of all ratings, and the top 10% of movies receive about 60%.
- At least a quarter of rated movies have only one rating, which limits collaborative filtering for those titles and motivates a content-based layer.

## Project Structure

```
├── backend/      API server (planned)
├── data/         Datasets (not tracked by Git)
├── frontend/     Web app (planned)
├── ml/           Reusable model and evaluation code
├── notebooks/    Exploration and experiments
└── README.md
```

## Data

Development uses the [MovieLens](https://grouplens.org/datasets/movielens/) "latest-small" dataset from GroupLens. Final evaluation results will use a stable benchmark version for reproducibility. Datasets are not included in this repository; download them from GroupLens and place them in the `data/` folder.

> F. Maxwell Harper and Joseph A. Konstan. 2015. The MovieLens Datasets: History and Context. *ACM Transactions on Interactive Intelligent Systems (TiiS)* 5, 4: 19:1–19:19.
