# Meta policy for selecting primitive skills in a complex environment
## Introduction
When people learn complex tasks, they tend to learn basic tasks first and perform complex tasks rather than learning the whole tasks altogether. Our idea is to apply this mechanism to reinforcement learning as well. For example, if you already have primitive policies for walking, jumping and crawling, we aim to create a policy for a complicated environment that can be divided by the three primitive actions with help of these primitive policies.
Our goal is to suggest a better way than previous research when training meta-policy. There is still much research in the field of meta-learning and there are no satisfactory results yet. There is a major off-policy problem in learning the meta-policy with reference to the default policies, and there is no specific guidance for dividing tasks.

## Related Work
There are many ways to learn continuous control which involves high dimensional action space in a complex environment. The most straightforward way is learning from the scratch. Because of the difficulty of the task, it usually requires complex reward shaping or curriculum learning(Heess et al., 2017). However, these methods have a disadvantage that since each trained tasks are not reusable, they have to learn from the beginning every time if another task comes out.
Another way to solve this problem by reusing primitive policies is using transition policy(Lee et al., 2019). By adding a transition policy prior to each primitive policy, complex tasks can be solved by composing primitives. However, it requires to decide the sequence of the primitive which is needed in a specific environment and takes a considerable amount of time to learn the transition policy for each primitive.
Our proposed methods aims to reuse pre-trained primitive policies and let meta-policy choose primitive policy according to the situation for N timesteps. Also, we want to connect different primitive policies smoothly by fine-tuning primitive policies.

## Approach
### Network structure and RL objective
Let’s denote meta-policy as 𝛳 and set of primitive policies as 𝜙 = {𝜙1, 𝜙2, 𝜙3, ...}. Based on current state, meta-policy selects a primitive policy between 𝜙. And selected primitive policy runs n steps to achieve RL object below (Figure 1). To train this network, we assume that when master policy changes a primitive policy to another primitive based on current state, the states have a temporal coherence. Our suggestion is that each primitive policy can be fine-tuned with the transition states. Since the transition states are similar each other, fine-tuning each primitive policy is possible. 
<p align="center">
  <img src="https://github.com/shashacks/master-robot/blob/master/images/equation.png" width="500" alt="test">
</p>

<p align="center">
  <img src="https://github.com/shashacks/master-robot/blob/master/images/equation.png" width="500" alt="{{ 11111111111111 }}">
  <figcaption>{{ 11111111111111 }}</figcaption>
</p>

## Experiments
## Conclusion
## References

## Usage
### walk
train
```
python -m rl.main --hrl false --prefix forward_v3_gear_hmap_5m --env Walker2dForwardHmap-v1 --is_train True
```
test
```
python -m rl.main --hrl false --prefix forward_v3_gear_hmap_5m --env Walker2dForwardHmap-v1 --is_train False --record True
```

### Crawl
train
```
python -m rl.main --hrl false --prefix crawl_v3_gear_hmap_5m --env Walker2dCrawlHmap-v1 --is_train True
```
test
```
python -m rl.main --hrl false --prefix crawl_v3_gear_hmap_5m --env Walker2dCrawlHmap-v1 --is_train False --record True
```

### Jump
train
```
python -m rl.main --hrl false --prefix jump_v3_gear_hmap_5m --env Walker2dJumpHmap-v1 --is_train True
```
test
```
python -m rl.main --hrl false --prefix jump_v3_gear_hmap_5m --env Walker2dJumpHmap-v1 --is_train False --record True
```

### Pre-training meta-policy and fine tuning
train
```
python -m rl.main --hrl True --is_train True --prefix obstacle --env Walker2dObstacleCourseHmap2-v1 --num_primitive 3 --primitive_envs Walker2dForwardHmap-v1,Walker2dCrawlHmap2-v1,Walker2dJumpHmap-v1 --primitive_prefixes forward_hmap_walker_v3_gear,crawl_hmap_gear_pass_12_2,jump_v3_gear_hmap_5m
```
test
```
python -m rl.main --hrl True --is_train False --prefix obstacle --env Walker2dObstacleCourseHmap2-v1 --num_primitive 3 --primitive_envs Walker2dForwardHmap-v1,Walker2dCrawlHmap2-v1,Walker2dJumpHmap-v1 --primitive_prefixes forward_hmap_walker_v3_gear,crawl_hmap_gear_pass_12_2,jump_v3_gear_hmap_5m --seed 236
```
