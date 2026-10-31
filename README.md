# Diagnosing Experimental Failures in A/B Testing

## Objective
I found and fixed common problems in A/B test analysis, including broken CUPED code, sample ratio mismatch, interference between users, Bayesian decision rules, and peeking.

## Methodology
- Fixed three bugs in an analyst's CUPED code: theta used the correlation instead of Cov/Var, the pre-period covariate was not centered, and the standard error was computed from the raw outcome instead of the adjusted one.
- Ran chi-squared sample ratio mismatch (SRM) tests on two experiments with a p < 0.001 alarm threshold, and compared the size of each imbalance with its test statistic.
- Simulated a two-sided marketplace with limited inventory, and compared the A/B test's naive ATE with the true individual effect and with a global rollout.
- Ran a Bayesian A/B test with a Beta-Binomial model and Beta(1, 1) priors, computed P(B > A) and the expected loss of shipping B, and compared the result with a permutation test.
- Wrote my own `bayes_ab()` function and used it on a test with four times the users.
- Simulated 1,000 experiments with no real effect, peeking at 50 points in each one, and compared the false positive rates with and without peeking.

## Key Findings
- **CUPED:** The analyst reported a 0.0% variance reduction. After my fixes it was 82.7%, and the SE fell from 0.1539 to 0.0641.
- **Marketplace interference:** The A/B test's naive ATE was 0.1861, but rolling the discount out to everyone gave 0.1428. The A/B test overstated the effect by 23.3% of the naive estimate.
- **Bayesian A/B test:** P(B > A) = 0.8866 and the expected loss of shipping B was 0.000473, so the ship rule said False. The permutation test gave p = 0.2302.
- **More data:** With four times the users, P(B > A) = 0.9951.
- **Peeking:** 32.4% of the no-effect experiments crossed p < 0.05 at least once, compared with 5.3% using a single test at the end.
