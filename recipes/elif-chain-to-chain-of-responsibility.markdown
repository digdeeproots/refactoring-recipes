# Convert Elif Chain into a Chain of Responsibility



## Recipe

Sequence:

1. Get one like we want to extract.
1. Extract the conditional.
1. Extract the body.
1. Extract conditional and body into a new step class.
1. Test the conditional.
1. Consider: should I test the body? Do it if easy.
1. Extract to a pipeline with that one step, that returns true if it handled the result.
1. Write 2 tests for pipeline: 1st step handles, no step handles.
1. Extract the pipeline to a field.
1. Remove any fields that you can.
1. Approve the set of steps in the pipeline.
1. Extract the second one. Might genericize the first step or create a new step.
1. Approve the pipeline change.
1. Write test for the pipeline: 2nd step handles.
1. Refactor pipeline from 2 steps to a sequence of steps.
