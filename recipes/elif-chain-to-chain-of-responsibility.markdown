# Convert Elif Chain into a Chain of Responsibility



## Recipe

Sequence:

1. Get one if block like we want to extract.
1. Extract the conditional.
1. Extract the body.
1. Extract conditional and body into a new `Step` class.
1. Test the conditional.
1. Consider: should I test the body? Do it if it's easy.
1. Extract to a `ChooserChain` that contains only the one step and returns true if it handled the result.
1. Write 2 tests for choser:
	* 1st step handles
	* no step handles.
1. Extract the chooser to a field, initialized in the constructor.
1. Remove any fields that you can.
1. Make the chooser public. Approve the set of steps in the chooser.
1. Extract the second one. You might genericize the first step or create a new step.
1. Add the step to the pipe. Approve the chooser change.
1. Write test for the chooser:
	* 2nd step handles.
1. Refactor chooser from 2 steps to a sequence of steps.
1. Extract all other steps, one at a time.
