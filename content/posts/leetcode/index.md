+++
title = "LeetCode Daily Notes"
date = "2025-05-03T11:12:30+08:00"
draft = false
tags = ["leetcode"]
+++
> 先把所有筆記都寫一起等哪天塞不下了再分開，練習用英文寫
## <span class="tag easy">Easy</span>

## <span class="tag medium">Medium</span>

### minimum-domino-rotations-for-equal-row
- [Link](https://leetcode.com/problems/minimum-domino-rotations-for-equal-row/)
<details>
<summary>My First Submission</summary>

```cpp
class Solution {
public:
    int minDominoRotations(vector<int>& tops, vector<int>& bottoms) {
        int c1=tops[0];
        int c2=bottoms[0];
        int len = tops.size();
        int count = 0;
        int min_swap = len;
        int idx=0;
        bool done = false;
        while(idx < len){
            if(tops[idx] != c1)
                if (bottoms[idx] != c1) break;
                else count++;
            idx++;
        }
        if(idx == len){
            done=true;
            if(count < min_swap)
                min_swap = count;
        }
        count=0;
        idx=0;
        while(idx < len){
            if(tops[idx] != c2)
                if(bottoms[idx] != c2) break;
                else count++;
            idx++;
        }
        if(idx == len){
            done=true;
            if(count < min_swap)
                min_swap = count;
        }
        count=0;
        idx=0;
        while(idx < len){
            if(bottoms[idx] != c2)
                if(tops[idx] != c2) break;
                else count++;
            idx++;
        }
        if(idx == len){
            done=true;
            if(count < min_swap)
                min_swap = count;
        }
        count=0;
        idx=0;
        while(idx < len){
            if(bottoms[idx] != c1)
                if(tops[idx] != c1) break;
                else count++;
            idx++;
        }
        if(idx == len){
            done=true;
            if(count < min_swap)
                min_swap = count;
        }
        count=0;
        idx=0;
        if(done) return min_swap;
        else return -1;
    }
};
```
</details> 

- **My result:** <span class="result ac">AC</span>  
- **Original Idea & Result Analyzing:** I think the 4-while-loops looks too ugly. I find that I can track the c1 and count swap_c1_top&swap_c1_bottom in the same iteration. [Source](https://leetcode.com/problems/minimum-domino-rotations-for-equal-row/solutions/6709103/3-different-kinds-of-solutions-beats-100)

## <span class="tag hard">Hard</span>

### maximum-number-of-tasks-you-can-assign 
- [Link](https://leetcode.com/problems/maximum-number-of-tasks-you-can-assign/) 

<details>
<summary>My First Submission</summary>

```cpp
class Solution {
public:
    int maxTaskAssign(vector<int>& tasks, vector<int>& workers, int pills, int strength) {
        sort(tasks.begin(), tasks.end(), greater<int>());
        sort(workers.begin(), workers.end(), greater<int>());
        int count_completed = 0;
        int first_assign_idx = 0;
        while(first_assign_idx < workers.size()
            && first_assign_idx < tasks.size()
            && workers[first_assign_idx] >= tasks[first_assign_idx])
            first_assign_idx++;
        if(first_assign_idx >= workers.size()
            || first_assign_idx >= tasks.size())
            return first_assign_idx;
        count_completed = first_assign_idx;
        int head_rest_workers = first_assign_idx;
        int head_rest_tasks = first_assign_idx;
        queue<int> q;
        while(head_rest_workers < workers.size()
            && head_rest_tasks < tasks.size()){
            if (workers[head_rest_workers] >= tasks[head_rest_tasks]){
                head_rest_workers++;
                head_rest_tasks++;
                count_completed++;
            }
            else{
                q.push(tasks[head_rest_tasks]);
                head_rest_tasks++;
            }
        }
        if (head_rest_workers >= workers.size()) return count_completed++;
        while (!q.empty() && pills > 0 && head_rest_workers < workers.size()){
            if (workers[head_rest_workers] + strength >= q.front()){
                head_rest_workers++;
                count_completed++;
                pills--;
            }
            q.pop();
        }
        return count_completed++;
    }
};
```
</details>

- **My result:** <span class="result wa">WA</span>  
- **Original Idea & Result Analyzing:** 
  - I tried to `find the first k strongest workers to complete k hardest tasks.` Once I find that there is a task that the worker cannot solve, I `push the task to a queue` because it can only be solved with pills, and then I check the next task. If `worker >= task[i], the worker is assigned to solve this task`, and we move to the next worker&task. Next, we would finish assigning workers or push all tasks to the queue, which means that even the current strongest worker cannot solve the easiest task. In the end, I check if the rest workers can solve tasks with pills.
  - I get `37 / 49 testcases passed`. I find that it is because I tried to assign stronger worker to a easier task in the loop that I check `worker >= task[i]`, but actually the worker may solve a harder task with pills.
- Community [Solution](https://leetcode.com/problems/maximum-number-of-tasks-you-can-assign/solutions/6703695/binary-search-greedy-with-images-example-walkthrough-c-python-java)
  - Idea: Binary search k, and check if k is **posible**
  - Correctness: doing...


