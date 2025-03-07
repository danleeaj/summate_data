> ⚠️ WARNING
> There was an issue with this experimental run that resulted in _bad data_.

#### Experiment 01
Date: March 6th, 2025

##### Goal
To determine if temperature has an effect on the variance of the results the autograder produced.

##### Methods
10 student responses were graded 15 times at 6 temperatures [0.0, 0.2, 0.4, 0.6, 0.8, 1.0].

##### Results
Running a simple linear regression on the variance (IQR) of the evaluations produced by the grader yielded a weak correlation (R squared = 0.0049). This raised some concerns becuase, theoretically, a temperature of 0 would result in the exact same output everytime - meaning no variance. However, we observed variance across the 15 evaluations.

![alt text](figure.png)
Relationship between temperature setting and interquartile range (IQR) of evaluation scores with 15 repeats. Points represent the mean IQR values at each temperature (0.0, 0.2, 0.4, 0.6, 0.8, and 1.0), with error bars showing standard error of mean.

##### Discussion
These results were not what we expected, and I would consider them faulty. To address this, for the next iteration of this experiment, I will include more debugging code (so each run would also output the temperature). What could be happening is that there is no temperature variance - even though I am asking the model to change temperature, because a previous model was already loaded, it was not updating the temperature.

##### Code
```python
reps = 15
temps = [0, 0.2, 0.4, 0.6, 0.8, 1]

total = reps * len(temps) * len(responses)

for rep in range(reps):

    for temp in temps:

        for i, response in enumerate(responses):

            print(f"Iteration {rep+1} of grading response {i+1} at {temp}")
            print(f"{total} evaluations left")

            filename = f"expt_temp_{temp}_resp_{i+1}_run_{rep+1}.txt"
            result = autograder.evaluate(student_response)
            with open(save_path + '/' + filename, 'w') as f:
                f.write(str(result.content) + '\n')
                f.write(str(result.stats))

            total -= 1
```
