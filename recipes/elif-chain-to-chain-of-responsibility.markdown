# Convert Elif Chain into a Chain of Responsibility



## Recipe

Sequence:

1. Get one if block like we want to extract. Do any code cleanup that is easier before separation.
1. Extract the conditional as `shouldHandle`.
1. Extract the body `handle`.
1. Extract `shouldHandle` and `handle` into a new `Step` class.
	* Leave per-call data for the `handle` and `shouldHandle` calls as parameters to those methods.
	* Pass in any long-lived data to the `Step` constructor. This might require moving fields over to the `Step` class from the outer class, or converting an argument for `handle` to a field in `Step` and then moving initialization.
1. Test `shouldHandle`.
1. Consider: is it easy to test `handle`? Do it if it's easy.
1. Extract the entire if block, but not the step instantiation, to an `applesauce` method.
1. Extract `applesauce` to a new `ChooserChain` class (using Extract Delegate) that contains only the one step and returns true if it handled the result.
	* The `Step` should be pass into the constructor. Move it from a parameter to a field if necessary.
	* You might be able to re-use a prior `ChooserChain` implementation (if you have one that is generic on arguments for `Step`). If so, you can skip all the steps that build `ChooserChain` up incrementally.
1. Write 2 tests for `ChooserChain`:
	* no step should handle (`shouldHandle` returns `false`) => chain does not call `handle` and returns `false`.
	* Since you don't want to use a real `Step` and its side effects just to test `ChooserChain`, extract `Step` to be an interface and rename the concrete class to match its purpose.
	* 1st step should handle (`shouldHandle` returns `true`) => chain calls `handle` on the step and returns `true`.
1. Extract the `ChooserChain` variable to a field, initialized in the constructor.
1. Remove any unused fields from the outer class.
1. Approve the set of steps in the chooser field.
	* You will have to make the chooser field public.
1. Extract the second if block, using steps 1 - 6 again.
	* You might genericize the first `Step` or create a new `Step`.
1. Add the step to the pipe. Approve the chooser change.
	* Just store the step in a second step field - no list yet.
	* `ChooserChain` should just ignore the second step - never check or run it.
	* Don't remove the code that uses the step directly! That code is the one actually performing the step.
	* Update the tests for `ChooserChain` to include a second step.
1. TDD a third test for `ChooserChain`:
	* 2nd step should handle (first step's `shouldHandle` returns `false` and second step's returns true) => chain calls `handle` on the second step and returns `true`.
	* Now delete the condition that you are extracting from the original method, as the `ChooserChain` will be handling that condition.
1. Refactor `ChooserChain` from 2 steps to a sequence of steps.
1. Extract all other steps, one at a time.
	1. Do steps 1 - 6.
	1. Move the `Step` instantiation to the constructor.
	1. Remove any unused fields.
	1. Add the step to the pipe and remove the if block from the original method.
1. Consider whether the outer class should exist. It might be better to have an init function that just creates the `ChooserChain`, and then have the outer class's existing callers just call the chooser's `handle` directly. Even if the class doesn't go away, the calling method might.
