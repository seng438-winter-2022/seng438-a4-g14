# Assignment 4

**SENG 438 - Software Testing, Reliability, and Quality**

**Lab. Report #4 – Mutation and GUI Testing**

| Group \#: 14   |   |
|----------------|---|
| Student Names: | Robert Brown (10180520) |
|                | Brooke Kindleman (30090660) |
|                | Risat Haque (30094174) |
|                | Amnah Hussain (30095907) | 


# 1 Introduction

In this lab, we first conducted mutation testing to the classes `Range` and `DataUtilities` from JFreeChart. Unlike the white box and black box testing from previous classes, we are instead inserting mutants and increasing the mutation score by at least `10%` for each of these classes. We initially started with `87%` and `69%` for `DataUtilities` and `Range` class. We utilized Pitest to help with mutation injection and provide mutation coverage based.  The second part of the lab included conducting GUI testing using the Selenium IDE. We tested various functionalities on the `[bestbuy.ca](http://bestbuy.ca)` website. We conducted tests on features such as logging in, utilizing the cart, interactive chat and other features. You will notice that GUI testing can present various faults if not conducted under specific conditions. For example, features that utilize Captcha’s will result in failure. This will be evident in the remainder of our report. 

# Analysis of 10 Mutants of the Range class

| Mutants | Survived/Killed | Analysis |
| --- | --- | --- |
| Function: Range expand(Range range, double lowerMargin, double upperMargin)
Mutant: removed call to org/jfree/chart/util/ParamChecks::nullNotPermitted | Killed | - killed due to a test made that puts a null range in
- called on paramchecks statement (line 359) |
| Function: boolean isNaNRange()
Mutant: Substituted 1 with -1 | Survived | - equivalent testing for return true part of function
- called on return true statement (line 482) |
| Function: int hashCode()
Mutant: Substituted 32 with -32 | Survived | - equivalent testing for shifting of bits
- called on result = (int) (temp ^ (temp >>> 32)) statement (line 500) |
| Function: boolean intersects(double b0, double b1)
Mutant: greater than to greater or equal | Killed | - previously survived now killed
- called on if (b0 <= this.lower) (line 170) |
| Function: Range combine(Range range1, Range range2)
Mutant: replaced call to java/lang/Math::max with argument | Killed | - previously survived now killed
- called on double u = Math.max(range1.getUpperBound(), range2.getUpperBound()); (line 249)
 |
| Function: Range shift(Range base, double delta)
Mutant: replaced call to org/jfree/data/Range::shift with argument | Killed | - previously had no coverage, now killed
- called on return shift(base, delta, false) (line 379) |
| Function: int hashCode()
Mutant: Replaced XOR with AND | Killed | - catches the change of xor to and, they are not logically the same
- called on result = (int) (temp ^ (temp >>> 32)); (line 500) |
| Function: int hashCode()
Mutant: Incremented (a++) integer local variable number 3 | Survived | - mutant survived on return result line
- equivalent mutant
- called on return result statement(line 503) |
| Function: Range expandToInclude(Range range, double value)
Mutant: not equal to equal | Killed | - called on if range == null statement (line 328) |
| Function: boolean intersects(double b0, double b1)
Mutant: replaced boolean return with true for org/jfree/data/Range::intersects | Killed | - mutant didn't exist before because newer version has adjusted source code.
- called on return false statement (line 180) |

# Report all the statistics and the mutation score for each test class

ADD PHOTOS OF BEFORE AND AFTER HERE

**DataUtilitiesTest**

|  | Before | After |
| --- | --- | --- |
| Survived | 92 | 39 |
| Killed | 595 | 648 |
| Total | 687 | 687 |
| Mutation Coverage Percentage | 87% | 94% |

**RangeTest**

|  | Before | After |
| --- | --- | --- |
| Survived | 392 | 281 |
| Killed | 867 | 1028 |
| Total | 1259 | 1309 |
| Mutation Coverage Percentage  | 69% | 79% |

# Analysis drawn on the effectiveness of each of the test classes

Both class Range and class DataUtilities were able to have their mutation coverage increased through the modification of both the test classes, as well as the source code itself. Range was able to see an increase in mutation coverage score from an initial 69% to a final 79%. This was accomplished through the modification of existing tests, such as the further granulation of test cases for `isNaNRange()`, where the previous tests included assertions for both the upper and lower bounds of the in the same case; each of these cases was split into two cases, one for upper, one for lower, in Lab 4, allowing for more mutants to be caught and for fewer issues to go undetected because of the test format. Changes to the source code to allow for better coverage of mutants involved methods such as increasing the abstraction of logical comparisons in `if else` statements. This decreased the chances of a mutation of one variable in a logical statement being performed in opposition to another variable in the same logical statement, cancelling each other out and effectively causing an equivalent mutation that provides the same outcome as non-mutated, correct version. Separating these logical statements into the individual variables greatly reduced the chances of this happening and facilitated the removal of several equivalent mutants without changing the functional operation of the source method itself.

Class DataUtilities was resolved in a similar method to class Range, but was not able to have its mutation coverage increased by 10% overall. Mutation coverage was initially at 87% and was able to be brought up to a final coverage of 94%, with the remaining 3% of mutations unable to be killed. Unlike Range, which began at only 69% mutation coverage, more than three quarters of DataUtilities mutations had already been caugth before new test cases for Lab 4 had been written, leaving very few mutants for modifications to the test suites and source code to be based on. Equivalent mutants, which did not influence the function of the methods tested, such as the mutation `Decremented (a--) double local variable number 1`, as found for source method `Range(double lower, double upper)` when the class member variables `this.upper` and `this.lower` are being assigned values before the method ends. The use of the decrement `a--` creates an mutant that cannot be killed, as the mutation occurs after the otherwise correct value assignment to the class member variables. Because of this post-assignment mutation, there is no valid way to rewrite the source code, as this action eventually must be performed so the instance of class Range can be created, nor can it be tested for, as the mutation occurs after the correct values have been assigned, making it appear in testing as though the method was not mutated in the first place. Issues involving the resolution of equivalent mutants limited the amount of progress able to be made in the increase of mutation coverage, especially for class DataUtilities.

### Original Test Suite for Test Class `Range` From Lab 3, Without Modifications for Mutation Coverage

```java
package org.jfree.data;

import static org.junit.Assert.*;
import org.jfree.data.Range;
import org.junit.*;

public class RangeTest {
    private Range exampleRange;
    private Range higherRange;
    private Range extraneousMaxRange;
    private Range extraneousMinRange;
    private Range refRange;
    private Range testRange;
    private Range smallestRange;
	private Range nanRange;

    @BeforeClass public static void setUpBeforeClass() throws Exception {
    }

    @Before
    public void setUp() throws Exception { 
    	exampleRange = new Range(-1, 1);
        higherRange = new Range(2,4);
        extraneousMaxRange = new Range(1, Double.MAX_VALUE);
        extraneousMinRange = new Range(-Double.MIN_VALUE, 1);
    	refRange = new Range (-8.0, 16.0);
    	smallestRange = new Range(0.0, Double.MIN_VALUE);
    }

	@Test(expected = IllegalArgumentException.class)
	public void rangeConstructorLowerBoundGreaterThanUpper() {
		testRange = new Range(1, -1);
	}

	@Test
	public void constrainValueAboveUpper() {
		assertEquals("The constrained value should be 1.0", 1.0, exampleRange.constrain(2.0), .000000001d);
	}

	@Test
	public void constrainValueBelowLower() {
		assertEquals("The constrained value should be -1.0", -1.0, exampleRange.constrain(-3.0), .000000001d);
	}

	@Test
	public void constrainContainsValue() {
		assertEquals("The constrained value should be 0.5", 0.5, exampleRange.constrain(0.5), .000000001d);
	}

	@Test
	public void combineNullAndValidRange() {
		testRange = Range.combine(null, exampleRange);
		assertEquals("The upper bound should be 1", 1, testRange.getUpperBound(), .000000001d);
        assertEquals("The lower bound should be -1", -1, testRange.getLowerBound(), .000000001d);
	}

	@Test
	public void combineNullAndInvalidRange() {
		testRange = Range.combine(higherRange, null);
		assertEquals("The upper bound should be 4", 4, testRange.getUpperBound(), .000000001d);
        assertEquals("The lower bound should be 4", 2, testRange.getLowerBound(), .000000001d);
	}

	@Test
	public void combineValidRanges() {
		testRange = Range.combine(exampleRange, higherRange);
		assertEquals("The upper bound should be 4", 4, testRange.getUpperBound(), .000000001d);
        assertEquals("The lower bound should be -1", -1, testRange.getLowerBound(), .000000001d);
	}

	@Test
	public void combineIgnoreNaNNullAndValidRange() {
		testRange = Range.combineIgnoringNaN(null, exampleRange);
		assertEquals("The upper bound should be 1", 1, testRange.getUpperBound(), .000000001d);
        assertEquals("The lower bound should be -1", -1, testRange.getLowerBound(), .000000001d);
	}

	@Test
	public void combineIgnoreNaNNullAndNanRange() {
		nanRange = new Range(Double.NaN, Double.NaN);
		testRange = Range.combineIgnoringNaN(null, nanRange);
		assertNull("The test range should be null", testRange);
	}

	@Test
	public void combineIgnoreNaNValidRangeAndNull() {
		testRange = Range.combineIgnoringNaN(higherRange, null);
		assertEquals("The upper bound should be 4", 4, testRange.getUpperBound(), .000000001d);
        assertEquals("The lower bound should be 4", 2, testRange.getLowerBound(), .000000001d);
	}

	@Test
	public void combineIgnoreNaNNaNAndNullRange() {
		nanRange = new Range(Double.NaN, Double.NaN);
		testRange = Range.combineIgnoringNaN(nanRange, null);
		assertNull("The test range should be null", testRange);
	}

	@Test
	public void combineIgnoreNaNValidRanges() {
		nanRange = new Range(Double.NaN, Double.NaN);
		testRange = Range.combineIgnoringNaN(exampleRange, higherRange);
		assertEquals("The upper bound should be 4", 4, testRange.getUpperBound(), .000000001d);
        assertEquals("The lower bound should be -1", -1, testRange.getLowerBound(), .000000001d);
	}

	@Test
	public void combineIgnoreNaNNaNRanges() {
		nanRange = new Range(Double.NaN, Double.NaN);
		testRange = Range.combineIgnoringNaN(nanRange, nanRange);
		assertNull("The test range should be null", testRange);
	}

	@Test
	public void expandUpperAboveLower() {
		testRange = Range.expand(higherRange, 2, 4);
		assertEquals("The upper bound should be 12.0", 12, testRange.getUpperBound(), .000000001d);
        assertEquals("The lower bound should be -2", -2, testRange.getLowerBound(), .000000001d);

	}

	@Test
	public void expandUpperBelowLower() {
		testRange = Range.expand(exampleRange, 4, -6);
		assertEquals("The upper bound should be -10", -10, testRange.getUpperBound(), .000000001d);
        assertEquals("The lower bound should be -10", -10, testRange.getLowerBound(), .000000001d);

	}

    @Test
    public void expandToIncludeValidRangeAboveRange() {
        Range testRange = Range.expandToInclude(exampleRange, 2);
        assertEquals("The upper bound should be 2", 2, testRange.getUpperBound(), .000000001d);
        assertEquals("The lower bound should be -1", -1, testRange.getLowerBound(), .000000001d);
    }

    @Test
    public void expandToIncludeValidRangeBelowRange() {
        Range testRange = Range.expandToInclude(exampleRange, -2);
        assertEquals("The upper bound should be 1", 1, testRange.getUpperBound(), .000000001d);
        assertEquals("The lower bound should be -2", -2, testRange.getLowerBound(), .000000001d);
    }

    @Test
    public void expandToIncludeValidRangeInRange() {
        Range testRange = Range.expandToInclude(exampleRange, 0);
        assertEquals("The upper bound should be 1", 1, testRange.getUpperBound(), .000000001d);
        assertEquals("The lower bound should be -1", -1, testRange.getLowerBound(), .000000001d);
    }

    @Test
    public void expandToIncludeValidRangeUpperBound() {
        Range testRange = Range.expandToInclude(exampleRange, 1);
        assertEquals("The upper bound should be 1", 1, testRange.getUpperBound(), .000000001d);
        assertEquals("The lower bound should be -1", -1, testRange.getLowerBound(), .000000001d);
    }

    @Test
    public void expandToIncludeValidRangeLowerBound() {
        Range testRange = Range.expandToInclude(exampleRange, -1);
        assertEquals("The upper bound should be 1", 1, testRange.getUpperBound(), .000000001d);
        assertEquals("The lower bound should be -1", -1, testRange.getLowerBound(), .000000001d);
    }
    
    @Test
    public void expandToIncludeNullRangeAboveRange() {
        Range testRange = Range.expandToInclude(null, 2);
        assertEquals("The upper bound should be 2", 2, testRange.getUpperBound(), .000000001d);
        assertEquals("The lower bound should be 2", 2, testRange.getLowerBound(), .000000001d);
    }

    @Test
    public void expandToIncludeValidRangeLargeExtraneousValue() {
        Range testRange = Range.expandToInclude(exampleRange, Double.MAX_VALUE);
        assertEquals("The upper bound of the range should be " + Double.toString(Double.MAX_VALUE), Double.MAX_VALUE, testRange.getUpperBound(), .000000001d);
    }

    @Test
    public void shiftValidRangeGreaterThanZero() {
        Range testRange = Range.shift(higherRange, 2, false);
        assertEquals("The upper bound should be 6", 6, testRange.getUpperBound(), .000000001d);
        assertEquals("The lower bound should be 4", 4, testRange.getLowerBound(), .000000001d);
    }
    
    @Test
    public void shiftValidRangeGreaterThanZeroTwoParams() {
        Range testRange = Range.shift(higherRange, 2, false);
        assertEquals("The upper bound should be 6", 6, testRange.getUpperBound(), .000000001d);
        assertEquals("The lower bound should be 4", 4, testRange.getLowerBound(), .000000001d);
    }

    @Test
    public void shiftValidRangeEqualsZero() {
        Range testRange = Range.shift(exampleRange, 0, false);
        assertEquals("The upper bound should be 1", 1, testRange.getUpperBound(), .000000001d);
        assertEquals("The lower bound should be -1", -1, testRange.getLowerBound(), .000000001d);
    }

    @Test
    public void shiftValidRangeLessThanZero() {
        Range testRange = Range.shift(higherRange, -1, false);
        assertEquals("The upper bound should be 3", 3, testRange.getUpperBound(), .000000001d);
        assertEquals("The lower bound should be 1", 1, testRange.getLowerBound(), .000000001d);
    }

	@Test
	public void shiftValidRangeGreaterThanZeroIncludesZeroBounds() {
		Range testRange = Range.shift(smallestRange, 1, false);
		assertEquals("The upper bound should be " + Double.toString(Double.MIN_VALUE + 1), Double.MIN_VALUE + 1, testRange.getUpperBound(), .000000001d);
        assertEquals("The lower bound should be 1", 1, testRange.getLowerBound(), .000000001d);
	}

	@Test
	public void shiftValidRangeGreaterThanZeroWithZeroCrossing() {
		Range testRange = Range.shift(exampleRange, 2, true);
		assertEquals("The upper bound should be 3.0", 3.0, testRange.getUpperBound(), .000000001d);
        assertEquals("The lower bound should be 1.0", 1.0, testRange.getLowerBound(), .000000001d);
	}

    @Test(expected = IllegalArgumentException.class)
    public void shiftNullRangeLessThanZero() {
        Range testRange = Range.shift(null, -2, false);
    }
    
    @Test
    public void shiftValidRangeMinNegativeDoubleValue() {
        Range testRange = Range.shift(exampleRange, -1 * Double.MIN_VALUE, false);
        assertEquals("The upper bound should be -1", -1, testRange.getLowerBound(), .000000001d);
    }

    @Test
    public void shiftValidRangeMaxDoubleValue() {
        Range testRange = Range.shift(exampleRange, Double.MAX_VALUE, false);
        assertEquals("The upper bound should be " + Double.toString(Double.MAX_VALUE + 1), Double.MAX_VALUE + 1, testRange.getUpperBound(), .000000001d);
        assertEquals("The lower bound should be 0", 0, testRange.getLowerBound(), .000000001d);
    }

    @Test
    public void shiftValidRangeCrossesZero() {
        Range testRange = Range.shift(exampleRange, 2, false);
        assertEquals("The upper bound should be 3", 3, testRange.getUpperBound(), .000000001d);
        assertEquals("The lower bound should be 0", 0, testRange.getLowerBound(), .000000001d);
    }

    @Test
    public void scaleValidRangeGreaterThanOne() {
        Range testRange = Range.scale(exampleRange, 2);
        assertEquals("The upper bound should be 2", 2, testRange.getUpperBound(), .000000001d);
        assertEquals("The lower bound should be -2", -2, testRange.getLowerBound(), .000000001d);
    }

    @Test
    public void scaleValidRangeEqualsOne() {
        Range testRange = Range.scale(exampleRange, 1);
        assertEquals("The upper bound should be 1", 1, testRange.getUpperBound(), .000000001d);
        assertEquals("The lower bound should be -1", -1, testRange.getLowerBound(), .000000001d);
    }

    @Test
    public void scaleValidRangeLessThanOne() {
        Range testRange = Range.scale(exampleRange, 0.5);
        assertEquals("The upper bound should be 0.5", 0.5, testRange.getUpperBound(), .000000001d);
        assertEquals("The lower bound should be -0.5", -0.5, testRange.getLowerBound(), .000000001d);
    }

    @Test(expected = IllegalArgumentException.class)
    public void scaleValidRangeLessThanZero() {
        Range testRange = Range.scale(exampleRange, -1);
    }

    @Test(expected = IllegalArgumentException.class)
    public void scaleNullRangeLessThanOne() {
        Range testRange = Range.scale(null, 2);
    }

    @Test
    public void scaleExtraneousMaxRangeGreaterThanOne() {
        Range testRange = Range.scale(extraneousMaxRange, 2);
        assertTrue("The upper bound of the range should be infinity", Double.isInfinite(testRange.getUpperBound()));
    }

    @Test
    public void scaleExtraneousMinRangeLessThanOne() {
        Range testRange = Range.scale(extraneousMinRange, 0.5);
        assertEquals("The lower bound of the range should be " + Double.toString(Double.MIN_VALUE / 2), Double.MIN_VALUE / 2, testRange.getLowerBound(), .000000001d);
    }

    @Test
    public void scaleValidRangeByMaxDouble() {
        Range testRange = Range.scale(exampleRange, Double.MAX_VALUE);
        assertEquals("The upper bound of the range should be " + Double.toString(Double.MAX_VALUE), Double.MAX_VALUE, testRange.getUpperBound(), .000000001d);
    }

    @Test
    public void scaleValidRangeByMinDouble() {
        Range testRange = Range.scale(exampleRange, Double.MIN_VALUE);
        assertEquals("The upper bound of the range should be " + Double.toString(Double.MIN_VALUE), Double.MIN_VALUE, testRange.getUpperBound(), .000000001d);
    }

	@Test
	public void intersectsWithinBoundaryNotTouchingBoundaryDoubles() {
		assertTrue("The range from -4.0 to 8.0 intersects the range from -8 to 16.", refRange.intersects(-4.0, 8.0));
	}

	@Test
	public void intersectsWithinBoundaryTouchingBoundaryDoubles() {
		assertTrue("The range from -8.0 to 16.0 intersects the range from -8.0 to 16.0",
				refRange.intersects(-8.0, 16.0));
	}

	@Test
	public void intersectsAboveUpperBoundaryAndBelowLowerBoundaryNotTouchingBoundaryDoubles() {
		assertTrue("The range from -28.0 to 36.0 intersects the range from -8.0 to 16.0",
				refRange.intersects(-8.0, 16.0));
	}

	@Test
	public void intersectsBelowLowerBoundaryAndWithinBoundaryDoubles() {
		assertTrue("The range from -14.0 to 2.0 intersects the range from -8.0 to 16.0",
				refRange.intersects(-14.0, 2.0));
	}

	// BUG: Should pass, because the test range overlaps with refRange at -8.0, but
	// the test fails instead
	// @Test
	// public void intersectsOnlyTouchingLowerBoundaryDoubles() {
	// 	assertTrue("The range from -14 to -8 intersects the range from -8.0 to 16.0", refRange.intersects(-14.0, -8.0));
	// }

	@Test
	public void intersectsFullyBelowLowerBoundaryNotTouchingBoundaryDoubles() {
		assertFalse("The range from -43.0 to -16.0 does not intersect the range from -8.0 to 16.0",
				refRange.intersects(-43.0, -16.0));
	}

	@Test
	public void intersectsAboveUpperBoundaryAndWithinBoundaryDoubles() {
		assertTrue("The range from 3.0 to 28.0 intersects the range from -8.0 to 16.0", refRange.intersects(3.0, 28.0));
	}

	// BUG: Should pass, because the test range overlaps with refRange at 16.0, but
	// the test fails instead
	//	@Test
	//	public void intersectsOnlyTouchingUpperBoundaryDoubles() {
	//		assertTrue("The range from 16.0 to 32.0 intersects the range from -8.0 to 16.0",
	//				refRange.intersects(16.0, 32.0));
	//	}

	@Test
	public void intersectsFullyAboveUpperBoundaryNotTouchingBoundaryDoubles() {
		assertFalse("The range from 17.0 to 19.0 does not intersect the range from -8.0 to 16.0",
				refRange.intersects(17.0, 19.0));
	}

	@Test
	public void interesectsMinDoubleValToMaxDoubleValDoubles() {
		assertTrue(
				"The range from MIN_VALUE for double to MAX_VALUE for double intersects with the range from -8.0 to 16.0",
				refRange.intersects(Double.MIN_VALUE, Double.MAX_VALUE));
	}

	@Test
	public void intersectSmallestPossibleRangeDoubles() {
		assertTrue("The range from 0.0 to MIN_VALUE for double intersects with the range from -8.0 to 16.0",
				refRange.intersects(0.0, Double.MIN_VALUE));
	}

	@Test
	public void intersectsWithinBoundaryNotTouchingBoundaryRangeArg() {
		testRange = new Range(2.0, 5.0);
		assertTrue("The range from 2.0 to 5.0 intersects the range from -8 to 16.", refRange.intersects(testRange));
	}

	@Test
	public void intersectsWithinBoundaryTouchingBoundaryRangeArg() {
		testRange = new Range(-8.0, 16.0);
		assertTrue("The range from -8.0 to 16.0 intersects the range from -8.0 to 16.0",
				refRange.intersects(testRange));
	}

	@Test
	public void intersectsAboveUpperBoundaryAndBelowLowerBoundaryNotTouchingBoundaryRangeArg() {
		testRange = new Range(-9.5, 17.5);
		assertTrue("The range from -9.5 to 17.5 intersects the range from -8.0 to 16.0",
				refRange.intersects(testRange));
	}

	@Test
	public void intersectsBelowLowerBoundaryAndWithinBoundaryRangeArg() {
		testRange = new Range(-10.0, 14.2);
		assertTrue("The range from -10.0 to 14.2 intersects the range from -8.0 to 16.0",
				refRange.intersects(testRange));
	}

	// BUG: Should pass, because the test range overlaps with refRange at -8.0, but
	// the test fails instead
	//	@Test
	//	public void intersectsOnlyTouchingLowerBoundaryRangeArg() {
	//		testRange = new Range(-45.0, -8.0);
	//		assertTrue("The range from -45.0 to -8.0 intersects the range from -8.0 to 16.0",
	//				refRange.intersects(testRange));
	//	}

	@Test
	public void intersectsFullyBelowLowerBoundaryNotTouchingBoundaryRangeArg() {
		testRange = new Range(-1004.0, -16.2);
		assertFalse("The range from -1004.0 to -16.2 does not intersect the range from -8.0 to 16.0",
				refRange.intersects(testRange));
	}

	@Test
	public void intersectsAboveUpperBoundaryAndWithinBoundaryRangeArg() {
		testRange = new Range(0.03, 17.7);
		assertTrue("The range from 0.03 to 17.7 intersects the range from -8.0 to 16.0",
				refRange.intersects(testRange));
	}

	// BUG: Should pass, because the test range overlaps with refRange at 16.0, but
	// the test fails instead
	//	@Test
	//	public void intersectsOnlyTouchingUpperBoundaryRangeArg() {
	//		testRange = new Range(16.0, 23.0);
	//		assertTrue("The range from 16.0 to 23.0 intersects the range from -8.0 to 16.0",
	//				refRange.intersects(testRange));
	//	}

	@Test
	public void intersectsFullyAboveUpperBoundaryNotTouchingBoundaryRangeArg() {
		testRange = new Range(200.75, 302.0);
		assertFalse("The range from 200.75 to 302.0 does not intersect the range from -8.0 to 16.0",
				refRange.intersects(testRange));
	}

	@Test
	public void intersectsMinDoubleValToMaxDoubleValRangeArg() {
		testRange = new Range(Double.MIN_VALUE, Double.MAX_VALUE);
		assertTrue(
				"The range from MIN_VALUE for double to MAX_VALUE for double intersects with the range from -8.0 to 16.0",
				refRange.intersects(testRange));
	}

	@Test
	public void intersectsInBoundsReverseValues() {
		assertFalse("Range intersects but provided range is invalid", higherRange.intersects(3, 2));
	}

	@Test
	public void intersectSmallestPossibleRangeArgs() {
		assertTrue("The range from 0.0 to MIN_VALUE for double intersects with the range from -8.0 to 16.0",
				refRange.intersects(smallestRange));
	}

	@Test
	public void containsBelowLowerBoundary() {
		assertFalse("The value -22. is not contained within the range from -8.0 to 16.0", refRange.contains(-22.0));
	}

	@Test
	public void containsOnLowerBoundary() {
		assertTrue("The value -8.0 is contained within the range from -8.0 to 16.0", refRange.contains(-8.0));
	}

	@Test
	public void containsBetweenUpperAndLowerBoundary() {
		assertTrue("The value 0.0 is contained within the range from -8.0 to 16.0", refRange.contains(0.0));
	}

	@Test
	public void containsOnUpperBoundary() {
		assertTrue("The value 16.0 is contained within the range from -8.0 to 16.0", refRange.contains(16.0));
	}

	@Test
	public void containsAboveUpperBoundary() {
		assertFalse("The value 99.0 is not contained within the range from -8.0 to 16.0", refRange.contains(99.0));
	}

	@Test
	public void containsMaxDoubleValue() {
		assertFalse("The maximum value of a double is not contained within the range from -8.0 to 16.0",
				refRange.contains(Double.MAX_VALUE));
	}

	@Test
	public void containsMinDoubleValue() {
		assertTrue("The minimum value of a double is contained within the range from -8.0 to 16.0",
				refRange.contains(Double.MIN_VALUE));
	}

	@Test
	public void getLengthTwo() {
		assertEquals("The length of the range should be " + 2.0, 2.0, exampleRange.getLength(), .000000001d);
	}

	@Test
	public void getCentralValueZero() {
		assertEquals("The central value of the range should be " + 0, 0, exampleRange.getCentralValue(), .000000001d);
	}

	@Test
	public void equalsInvalidRange() {
		String test = "testing";
		assertFalse("The comparator range is not an object of type range", exampleRange.equals(test));
	}

	@Test
	public void equalsSameUpperDifferentLower() {
		testRange = new Range(-2, 1);
		assertFalse("The ranges have different lower bounds", exampleRange.equals(testRange));
	}

	@Test
	public void equalsDifferentUpperSameLower() {
		testRange = new Range(-1, 2);
		assertFalse("The ranges have different upper bounds", exampleRange.equals(testRange));
	}

	@Test
	public void equalsSameUpperSameLower() {
		testRange = new Range(-1, 1);
		assertTrue("The ranges have the same bounds", exampleRange.equals(testRange));
	}

	@Test
	public void isNanRangeValidLowerInvalidUpper() {
		nanRange = new Range(-1, Double.NaN);
		assertFalse("The range only has an upper value that is NaN", nanRange.isNaNRange());
	}

	@Test
	public void isNanRangeInvalidLowerValidUpper() {
		nanRange = new Range(Double.NaN, 1);
		assertFalse("The range only has an lower value that is NaN", nanRange.isNaNRange());
	}

	@Test
	public void isNanRangeInvalidLowerInvalidUpper() {
		nanRange = new Range(Double.NaN, Double.NaN);
		assertTrue("The range has an upper and lower value that is NaN", nanRange.isNaNRange());
	}

	@Test
	public void isNanRangeValidLowerValidUpper() {
		assertFalse("The range has an upper value that is NaN", exampleRange.isNaNRange());
	}

	@Test
	public void toStringKnownRange() {
		String test = exampleRange.toString();
		assertEquals("Should return 'Range[-1.0,1.0]'", test.toString(), "Range[-1.0,1.0]");
	}

	@After
	public void tearDown() throws Exception {
	}

	@AfterClass
	public static void tearDownAfterClass() throws Exception {
	}
}
```

### Updated Test Suite for Test Class `Range` From Lab 4, With Modifications for Mutation Coverage

```java
package org.jfree.data;

import static org.junit.Assert.*;
import org.jfree.data.Range;
import org.junit.*;

public class RangeTest {
	private Range exampleRange;
	private Range higherRange;
	private Range extraneousMaxRange;
	private Range extraneousMinRange;
	private Range refRange;
	private Range testRange;
	private Range smallestRange;
	private Range fracRange;
	private Range nanRange;

	@BeforeClass
	public static void setUpBeforeClass() throws Exception {
	}

	@Before
	public void setUp() throws Exception {
		exampleRange = new Range(-1, 1);
		higherRange = new Range(2, 4);
		extraneousMaxRange = new Range(1, Double.MAX_VALUE);
		extraneousMinRange = new Range(-Double.MIN_VALUE, 1);
		refRange = new Range(-8.0, 16.0);
		smallestRange = new Range(0.0, Double.MIN_VALUE);
		fracRange = new Range(3.221, 7.8);
	}

	@Test(expected = IllegalArgumentException.class)
	public void rangeConstructorLowerBoundGreaterThanUpper() {
		testRange = new Range(1, -1);
	}

	@Test
	public void rangeValidConstructionCheckLower() {
		Range constructRange = new Range(-9.0, 2.0);
		assertTrue(
				"The lower value of a constructed range with arguments (-9.0, 2.0) matches the value '2.0', provided by the arguments.",
				(constructRange.getLowerBound() == -9.0));
	}

	@Test
	public void rangeValidConstructionCheckUpper() {
		Range constructRange = new Range(-9.0, 2.0);
		assertTrue(
				"The upper value of a constructed range with arguments (-9.0, 2.0) matches the value '-9.0', as provided by the arguments",
				(constructRange.getUpperBound() == 2.0));
	}

	@Test
	public void constrainValueAboveUpper() {
		assertEquals("The constrained value should be 1.0", 1.0, exampleRange.constrain(2.0), .000000001d);
	}

	@Test
	public void constrainValueBelowLower() {
		assertEquals("The constrained value should be -1.0", -1.0, exampleRange.constrain(-3.0), .000000001d);
	}

	@Test
	public void constrainContainsValue() {
		assertEquals("The constrained value should be 0.5", 0.5, exampleRange.constrain(0.5), .000000001d);
	}

	@Test
	public void combineNullAndValidRange() {
		testRange = Range.combine(null, exampleRange);
		assertEquals("The upper bound should be 1", 1, testRange.getUpperBound(), .000000001d);
		assertEquals("The lower bound should be -1", -1, testRange.getLowerBound(), .000000001d);
	}

	@Test
	public void combineNullAndInvalidRange() {
		testRange = Range.combine(higherRange, null);
		assertEquals("The upper bound should be 4", 4, testRange.getUpperBound(), .000000001d);
		assertEquals("The lower bound should be 4", 2, testRange.getLowerBound(), .000000001d);
	}

	@Test
	public void combineComparingIgnoreNaNandRegularOverload(){
	    Range testRange = new Range(1, 22.0);
	    assertTrue("\"combine()\" and \"combineIgonringNaN()\" return the same result when NaN is not used and the same arguments are provided.", Range.combine(testRange, exampleRange).equals(Range.combineIgnoringNaN(testRange, exampleRange)));
	}

	@Test
	public void combineValidRanges() {
		testRange = Range.combine(exampleRange, higherRange);
		assertEquals("The upper bound should be 4", 4, testRange.getUpperBound(), .000000001d);
		assertEquals("The lower bound should be -1", -1, testRange.getLowerBound(), .000000001d);
	}

	@Test
	public void combineIgnoreNaNNullAndValidRange() {
		testRange = Range.combineIgnoringNaN(null, exampleRange);
		assertEquals("The upper bound should be 1", 1, testRange.getUpperBound(), .000000001d);
		assertEquals("The lower bound should be -1", -1, testRange.getLowerBound(), .000000001d);
	}

	@Test
	public void combineIgnoreNaNNullAndNanRange() {
		nanRange = new Range(Double.NaN, Double.NaN);
		testRange = Range.combineIgnoringNaN(null, nanRange);
		assertNull("The test range should be null", testRange);
	}

	@Test
	public void combineIgnoreNaNValidRangeAndNull() {
		testRange = Range.combineIgnoringNaN(higherRange, null);
		assertEquals("The upper bound should be 4", 4, testRange.getUpperBound(), .000000001d);
		assertEquals("The lower bound should be 4", 2, testRange.getLowerBound(), .000000001d);
	}

	@Test
	public void combineIgnoreNaNNaNAndNullRange() {
		nanRange = new Range(Double.NaN, Double.NaN);
		testRange = Range.combineIgnoringNaN(nanRange, null);
		assertNull("The test range should be null", testRange);
	}

	@Test
	public void combineIgnoreNaNValidRanges() {
		nanRange = new Range(Double.NaN, Double.NaN);
		testRange = Range.combineIgnoringNaN(exampleRange, higherRange);
		assertEquals("The upper bound should be 4", 4, testRange.getUpperBound(), .000000001d);
		assertEquals("The lower bound should be -1", -1, testRange.getLowerBound(), .000000001d);
	}

	@Test
	public void combineIgnoreNaNNaNRanges() {
		nanRange = new Range(Double.NaN, Double.NaN);
		testRange = Range.combineIgnoringNaN(nanRange, nanRange);
		assertNull("The test range should be null", testRange);
	}

	@Test
	public void expandUpperAboveLower() {
		testRange = Range.expand(higherRange, 2, 4);
		assertEquals("The upper bound should be 12.0", 12, testRange.getUpperBound(), .000000001d);
		assertEquals("The lower bound should be -2", -2, testRange.getLowerBound(), .000000001d);

	}

	@Test
	public void expandUpperBelowLower() {
		testRange = Range.expand(exampleRange, 4, -6);
		assertEquals("The upper bound should be -10", -10, testRange.getUpperBound(), .000000001d);
		assertEquals("The lower bound should be -10", -10, testRange.getLowerBound(), .000000001d);

	}

	@Test
	public void expandToIncludeValidRangeAboveRange() {
		Range testRange = Range.expandToInclude(exampleRange, 2);
		assertEquals("The upper bound should be 2", 2, testRange.getUpperBound(), .000000001d);
		assertEquals("The lower bound should be -1", -1, testRange.getLowerBound(), .000000001d);
	}

	@Test
	public void expandToIncludeValidRangeBelowRange() {
		Range testRange = Range.expandToInclude(exampleRange, -2);
		assertEquals("The upper bound should be 1", 1, testRange.getUpperBound(), .000000001d);
		assertEquals("The lower bound should be -2", -2, testRange.getLowerBound(), .000000001d);
	}

	@Test
	public void expandToIncludeValidRangeInRange() {
		Range testRange = Range.expandToInclude(exampleRange, 0);
		assertEquals("The upper bound should be 1", 1, testRange.getUpperBound(), .000000001d);
		assertEquals("The lower bound should be -1", -1, testRange.getLowerBound(), .000000001d);
	}

	@Test
	public void expandToIncludeValidRangeUpperBound() {
		Range testRange = Range.expandToInclude(exampleRange, 1);
		assertEquals("The upper bound should be 1", 1, testRange.getUpperBound(), .000000001d);
		assertEquals("The lower bound should be -1", -1, testRange.getLowerBound(), .000000001d);
	}

	@Test
	public void expandToIncludeValidRangeLowerBound() {
		Range testRange = Range.expandToInclude(exampleRange, -1);
		assertEquals("The upper bound should be 1", 1, testRange.getUpperBound(), .000000001d);
		assertEquals("The lower bound should be -1", -1, testRange.getLowerBound(), .000000001d);
	}

	@Test
	public void expandToIncludeNullRangeAboveRange() {
		Range testRange = Range.expandToInclude(null, 2);
		assertEquals("The upper bound should be 2", 2, testRange.getUpperBound(), .000000001d);
		assertEquals("The lower bound should be 2", 2, testRange.getLowerBound(), .000000001d);
	}

	@Test
	public void expandToIncludeValidRangeLargeExtraneousValue() {
		Range testRange = Range.expandToInclude(exampleRange, Double.MAX_VALUE);
		assertEquals("The upper bound of the range should be " + Double.toString(Double.MAX_VALUE), Double.MAX_VALUE,
				testRange.getUpperBound(), .000000001d);
	}

	@Test
	public void shiftValidRangeGreaterThanZero() {
		Range testRange = Range.shift(higherRange, 2, false);
		assertEquals("The upper bound should be 6", 6, testRange.getUpperBound(), .000000001d);
		assertEquals("The lower bound should be 4", 4, testRange.getLowerBound(), .000000001d);
	}

	@Test
	public void shiftValidRangeGreaterThanZeroTwoParams() {
		Range testRange = Range.shift(higherRange, 2, false);
		assertEquals("The upper bound should be 6", 6, testRange.getUpperBound(), .000000001d);
		assertEquals("The lower bound should be 4", 4, testRange.getLowerBound(), .000000001d);
	}

	@Test
	public void shiftValidRangeEqualsZero() {
		Range testRange = Range.shift(exampleRange, 0, false);
		assertEquals("The upper bound should be 1", 1, testRange.getUpperBound(), .000000001d);
		assertEquals("The lower bound should be -1", -1, testRange.getLowerBound(), .000000001d);
	}

	@Test
	public void shiftValidRangeLessThanZeroUpperBound() {
		Range testRange = Range.shift(higherRange, -1, false);
		assertEquals("The upper bound should be 3", 3, testRange.getUpperBound(), .000000001d);
	}
	
	@Test
	public void shiftValidRangeLessThanZeroLowerBound() {
		Range testRange = Range.shift(higherRange, -1, false);
		assertEquals("The lower bound should be 1", 1, testRange.getLowerBound(), .000000001d);
	}

	@Test
	public void shiftValidRangeGreaterThanZeroIncludesZeroBoundsUpperBound() {
		Range testRange = Range.shift(smallestRange, 1, false);
		assertEquals("The upper bound should be " + Double.toString(Double.MIN_VALUE + 1), Double.MIN_VALUE + 1,
				testRange.getUpperBound(), .000000001d);
	}
	
	@Test
	public void shiftValidRangeGreaterThanZeroIncludesZeroBoundsLowerBound() {
		Range testRange = Range.shift(smallestRange, 1, false);
		assertEquals("The lower bound should be 1", 1, testRange.getLowerBound(), .000000001d);
	}

	@Test
	public void shiftValidRangeGreaterThanZeroWithZeroCrossingUpperBound() {
		Range testRange = Range.shift(exampleRange, 2, true);
		assertEquals("The upper bound should be 3.0", 3.0, testRange.getUpperBound(), .000000001d);
	}

	@Test
	public void shiftValidRangeGreaterThanZeroWithZeroCrossingLowerBound() {
		Range testRange = Range.shift(exampleRange, 2, true);
		assertEquals("The lower bound should be 1.0", 1.0, testRange.getLowerBound(), .000000001d);
	}
	
	@Test(expected = IllegalArgumentException.class)
	public void shiftNullRangeLessThanZero() {
		Range testRange = Range.shift(null, -2, false);
	}

	@Test
	public void shiftValidRangeMinNegativeDoubleValue() {
		Range testRange = Range.shift(exampleRange, -1 * Double.MIN_VALUE, false);
		assertEquals("The upper bound should be -1", -1, testRange.getLowerBound(), .000000001d);
	}

	@Test
	public void shiftValidRangeMaxDoubleValueUpperBound() {
		Range testRange = Range.shift(exampleRange, Double.MAX_VALUE, false);
		assertEquals("The upper bound should be " + Double.toString(Double.MAX_VALUE + 1), Double.MAX_VALUE + 1,
				testRange.getUpperBound(), .000000001d);
	}
	
	@Test
	public void shiftValidRangeMaxDoubleValueLowerBound() {
		Range testRange = Range.shift(exampleRange, Double.MAX_VALUE, false);
		assertEquals("The lower bound should be 0", 0, testRange.getLowerBound(), .000000001d);
	}

	@Test
	public void shiftValidRangeCrossesZeroUpperBound() {
		Range testRange = Range.shift(exampleRange, 2, false);
		assertEquals("The upper bound should be 3", 3, testRange.getUpperBound(), .000000001d);
	}
@Test
	public void shiftValidRangeCrossesZeroLowerBound() {
		Range testRange = Range.shift(exampleRange, 2, false);
		assertEquals("The lower bound should be 0", 0, testRange.getLowerBound(), .000000001d);
	}

	@Test
	public void scaleValidRangeGreaterThanOneUpperBound() {
		Range testRange = Range.scale(exampleRange, 2);
		assertEquals("The upper bound should be 2", 2, testRange.getUpperBound(), .000000001d);
	}
	
	@Test
	public void scaleValidRangeGreaterThanOnLowerBound() {
		Range testRange = Range.scale(exampleRange, 2);
		assertEquals("The lower bound should be -2", -2, testRange.getLowerBound(), .000000001d);
	}

	@Test
	public void scaleValidRangeEqualsOneUpperBound() {
		Range testRange = Range.scale(exampleRange, 1);
		assertEquals("The upper bound should be 1", 1, testRange.getUpperBound(), .000000001d);
	}
	
	@Test
	public void scaleValidRangeEqualsOneLowerBound() {
		Range testRange = Range.scale(exampleRange, 1);
		assertEquals("The lower bound should be -1", -1, testRange.getLowerBound(), .000000001d);
	}

	@Test
	public void scaleValidRangeLessThanOneUpperBound() {
		Range testRange = Range.scale(exampleRange, 0.5);
		assertEquals("The upper bound should be 0.5", 0.5, testRange.getUpperBound(), .000000001d);
	}

	
	@Test
	public void scaleValidRangeLessThanOneLowerBound() {
		Range testRange = Range.scale(exampleRange, 0.5);
		assertEquals("The lower bound should be -0.5", -0.5, testRange.getLowerBound(), .000000001d);
	}

	@Test(expected = IllegalArgumentException.class)
	public void scaleValidRangeLessThanZero() {
		Range testRange = Range.scale(exampleRange, -1);
	}

	@Test(expected = IllegalArgumentException.class)
	public void scaleNullRangeLessThanOne() {
		Range testRange = Range.scale(null, 2);
	}

	@Test
	public void scaleExtraneousMaxRangeGreaterThanOne() {
		Range testRange = Range.scale(extraneousMaxRange, 2);
		assertTrue("The upper bound of the range should be infinity", Double.isInfinite(testRange.getUpperBound()));
	}

	@Test
	public void scaleExtraneousMinRangeLessThanOne() {
		Range testRange = Range.scale(extraneousMinRange, 0.5);
		assertEquals("The lower bound of the range should be " + Double.toString(Double.MIN_VALUE / 2),
				Double.MIN_VALUE / 2, testRange.getLowerBound(), .000000001d);
	}

	@Test
	public void scaleValidRangeByMaxDouble() {
		Range testRange = Range.scale(exampleRange, Double.MAX_VALUE);
		assertEquals("The upper bound of the range should be " + Double.toString(Double.MAX_VALUE), Double.MAX_VALUE,
				testRange.getUpperBound(), .000000001d);
	}

	@Test
	public void scaleValidRangeByMinDouble() {
		Range testRange = Range.scale(exampleRange, Double.MIN_VALUE);
		assertEquals("The upper bound of the range should be " + Double.toString(Double.MIN_VALUE), Double.MIN_VALUE,
				testRange.getUpperBound(), .000000001d);
	}

	@Test
	public void intersectsWithinBoundaryNotTouchingBoundaryDoubles() {
		assertTrue("The range from -4.0 to 8.0 intersects the range from -8 to 16.", refRange.intersects(-4.0, 8.0));
	}

	@Test
	public void intersectsWithinBoundaryTouchingBoundaryDoubles() {
		assertTrue("The range from -8.0 to 16.0 intersects the range from -8.0 to 16.0",
				refRange.intersects(-8.0, 16.0));
	}

	@Test
	public void intersectsAboveUpperBoundaryAndBelowLowerBoundaryNotTouchingBoundaryDoubles() {
		assertTrue("The range from -28.0 to 36.0 intersects the range from -8.0 to 16.0",
				refRange.intersects(-8.0, 16.0));
	}

	@Test
	public void intersectsBelowLowerBoundaryAndWithinBoundaryDoubles() {
		assertTrue("The range from -14.0 to 2.0 intersects the range from -8.0 to 16.0",
				refRange.intersects(-14.0, 2.0));
	}

	// BUG: Should pass, because the test range overlaps with refRange at -8.0, but
	// the test fails instead
	// @Test
	// public void intersectsOnlyTouchingLowerBoundaryDoubles() {
	// assertTrue("The range from -14 to -8 intersects the range from -8.0 to 16.0",
	// refRange.intersects(-14.0, -8.0));
	// }

	@Test
	public void intersectsFullyBelowLowerBoundaryNotTouchingBoundaryDoubles() {
		assertFalse("The range from -43.0 to -16.0 does not intersect the range from -8.0 to 16.0",
				refRange.intersects(-43.0, -16.0));
	}

	@Test
	public void intersectsAboveUpperBoundaryAndWithinBoundaryDoubles() {
		assertTrue("The range from 3.0 to 28.0 intersects the range from -8.0 to 16.0", refRange.intersects(3.0, 28.0));
	}

	// BUG: Should pass, because the test range overlaps with refRange at 16.0, but
	// the test fails instead
	// @Test
	// public void intersectsOnlyTouchingUpperBoundaryDoubles() {
	// assertTrue("The range from 16.0 to 32.0 intersects the range from -8.0 to
	// 16.0",
	// refRange.intersects(16.0, 32.0));
	// }

	@Test
	public void intersectsFullyAboveUpperBoundaryNotTouchingBoundaryDoubles() {
		assertFalse("The range from 17.0 to 19.0 does not intersect the range from -8.0 to 16.0",
				refRange.intersects(17.0, 19.0));
	}

	@Test
	public void interesectsMinDoubleValToMaxDoubleValDoubles() {
		assertTrue(
				"The range from MIN_VALUE for double to MAX_VALUE for double intersects with the range from -8.0 to 16.0",
				refRange.intersects(Double.MIN_VALUE, Double.MAX_VALUE));
	}

	@Test
	public void intersectSmallestPossibleRangeDoubles() {
		assertTrue("The range from 0.0 to MIN_VALUE for double intersects with the range from -8.0 to 16.0",
				refRange.intersects(0.0, Double.MIN_VALUE));
	}

	@Test
	public void intersectsWithinBoundaryNotTouchingBoundaryRangeArg() {
		testRange = new Range(2.0, 5.0);
		assertTrue("The range from 2.0 to 5.0 intersects the range from -8 to 16.", refRange.intersects(testRange));
	}

	@Test
	public void intersectsWithinBoundaryTouchingBoundaryRangeArg() {
		testRange = new Range(-8.0, 16.0);
		assertTrue("The range from -8.0 to 16.0 intersects the range from -8.0 to 16.0",
				refRange.intersects(testRange));
	}

	@Test
	public void intersectsAboveUpperBoundaryAndBelowLowerBoundaryNotTouchingBoundaryRangeArg() {
		testRange = new Range(-9.5, 17.5);
		assertTrue("The range from -9.5 to 17.5 intersects the range from -8.0 to 16.0",
				refRange.intersects(testRange));
	}

	@Test
	public void intersectsBelowLowerBoundaryAndWithinBoundaryRangeArg() {
		testRange = new Range(-10.0, 14.2);
		assertTrue("The range from -10.0 to 14.2 intersects the range from -8.0 to 16.0",
				refRange.intersects(testRange));
	}

	// BUG: Should pass, because the test range overlaps with refRange at -8.0, but
	// the test fails instead
	// @Test
	// public void intersectsOnlyTouchingLowerBoundaryRangeArg() {
	// testRange = new Range(-45.0, -8.0);
	// assertTrue("The range from -45.0 to -8.0 intersects the range from -8.0 to
	// 16.0",
	// refRange.intersects(testRange));
	// }

	@Test
	public void intersectsFullyBelowLowerBoundaryNotTouchingBoundaryRangeArg() {
		testRange = new Range(-1004.0, -16.2);
		assertFalse("The range from -1004.0 to -16.2 does not intersect the range from -8.0 to 16.0",
				refRange.intersects(testRange));
	}

	@Test
	public void intersectsAboveUpperBoundaryAndWithinBoundaryRangeArg() {
		testRange = new Range(0.03, 17.7);
		assertTrue("The range from 0.03 to 17.7 intersects the range from -8.0 to 16.0",
				refRange.intersects(testRange));
	}

	// BUG: Should pass, because the test range overlaps with refRange at 16.0, but
	// the test fails instead
	// @Test
	// public void intersectsOnlyTouchingUpperBoundaryRangeArg() {
	// testRange = new Range(16.0, 23.0);
	// assertTrue("The range from 16.0 to 23.0 intersects the range from -8.0 to
	// 16.0",
	// refRange.intersects(testRange));
	// }

	@Test
	public void intersectsFullyAboveUpperBoundaryNotTouchingBoundaryRangeArg() {
		testRange = new Range(200.75, 302.0);
		assertFalse("The range from 200.75 to 302.0 does not intersect the range from -8.0 to 16.0",
				refRange.intersects(testRange));
	}

	@Test
	public void intersectsMinDoubleValToMaxDoubleValRangeArg() {
		testRange = new Range(Double.MIN_VALUE, Double.MAX_VALUE);
		assertTrue(
				"The range from MIN_VALUE for double to MAX_VALUE for double intersects with the range from -8.0 to 16.0",
				refRange.intersects(testRange));
	}

	@Test
	public void intersectsInBoundsReverseValues() {
		assertFalse("Range intersects but provided range is invalid", higherRange.intersects(3, 2));
	}

	@Test
	public void intersectSmallestPossibleRangeArgs() {
		assertTrue("The range from 0.0 to MIN_VALUE for double intersects with the range from -8.0 to 16.0",
				refRange.intersects(smallestRange));
	}

	@Test
	public void containsBelowLowerBoundary() {
		assertFalse("The value -22. is not contained within the range from -8.0 to 16.0", refRange.contains(-22.0));
	}

	@Test
	public void containsOnLowerBoundary() {
		assertTrue("The value -8.0 is contained within the range from -8.0 to 16.0", refRange.contains(-8.0));
	}

	@Test
	public void containsBetweenUpperAndLowerBoundary() {
		assertTrue("The value 0.0 is contained within the range from -8.0 to 16.0", refRange.contains(0.0));
	}

	@Test
	public void containsOnUpperBoundary() {
		assertTrue("The value 16.0 is contained within the range from -8.0 to 16.0", refRange.contains(16.0));
	}

	@Test
	public void containsAboveUpperBoundary() {
		assertFalse("The value 99.0 is not contained within the range from -8.0 to 16.0", refRange.contains(99.0));
	}

	@Test
	public void containsMaxDoubleValue() {
		assertFalse("The maximum value of a double is not contained within the range from -8.0 to 16.0",
				refRange.contains(Double.MAX_VALUE));
	}

	@Test
	public void containsMinDoubleValue() {
		assertTrue("The minimum value of a double is contained within the range from -8.0 to 16.0",
				refRange.contains(Double.MIN_VALUE));
	}

	@Test
	public void getLengthTwo() {
		assertEquals("The length of the range should be " + 2.0, 2.0, exampleRange.getLength(), .000000001d);
	}

	@Test
	public void getCentralValueZero() {
		assertEquals("The central value of the range should be " + 0, 0, exampleRange.getCentralValue(), .000000001d);
	}

	@Test
	public void getCentralValueFractionalResult() {
		assertEquals("The central value of the range should be" + 5.5105, 5.5105, fracRange.getCentralValue(),
				.000000001d);
	}

	@Test
	public void equalsInvalidRange() {
		String test = "testing";
		assertFalse("The comparator range is not an object of type range", exampleRange.equals(test));
	}

	@Test
	public void equalsSameUpperDifferentLower() {
		testRange = new Range(-2, 1);
		assertFalse("The ranges have different lower bounds", exampleRange.equals(testRange));
	}

	@Test
	public void equalsDifferentUpperSameLower() {
		testRange = new Range(-1, 2);
		assertFalse("The ranges have different upper bounds", exampleRange.equals(testRange));
	}

	@Test
	public void equalsSameUpperSameLower() {
		testRange = new Range(-1, 1);
		assertTrue("The ranges have the same bounds", exampleRange.equals(testRange));
	}

	@Test
	public void isNanRangeValidLowerInvalidUpper() {
		nanRange = new Range(-1, Double.NaN);
		assertFalse("The range only has an upper value that is NaN", nanRange.isNaNRange());
	}

	@Test
	public void isNanRangeInvalidLowerValidUpper() {
		nanRange = new Range(Double.NaN, 1);
		assertFalse("The range only has an lower value that is NaN", nanRange.isNaNRange());
	}

	@Test
	public void isNanRangeInvalidLowerInvalidUpper() {
		nanRange = new Range(Double.NaN, Double.NaN);
		assertTrue("The range has an upper and lower value that is NaN", nanRange.isNaNRange());
	}

	@Test
	public void isNanRangeValidLowerValidUpper() {
		assertFalse("The range has an upper value that is NaN", exampleRange.isNaNRange());
	}

	@Test
	public void toStringKnownRange() {
		String test = exampleRange.toString();
		assertEquals("Should return 'Range[-1.0,1.0]'", test.toString(), "Range[-1.0,1.0]");
	}

	//mutations
	//hash code
	
	@Test
	public void hash_ZeroValues() {
		Range testRange = new Range(0, 0);
		assertEquals(testRange.hashCode(), 0.0, .000000001d);
	}
	
	@Test
	public void hash_ValidValues() {
		Range testRange = new Range(4, 5);
		assertEquals(testRange.hashCode(), -2.115764224E9, .000000001d);
	}
	
	@Test
	public void hash_NegativeValidValues() {
		Range testRange = new Range(-5, -2);
		assertEquals(testRange.hashCode(), -2.109472768E9, .000000001d);
	}
	
	@Test
	public void hash_MixValidValues() {
		Range testRange = new Range(-5, 10);
		assertEquals(testRange.hashCode(), 4.0370176E7, .000000001d);
	}
	
	@Test(expected = NullPointerException.class)
	public void hash_Null() {
		Range testRange = null;
		testRange.hashCode();
	}
	
	@Test
	public void hash_Compare() {
		Range testRange1 = new Range(3, 12);
		Range testRange2 = new Range(3, 12);
		assertEquals(testRange1, testRange2);
	}
	
	@Test(expected = IllegalArgumentException.class)
	public void hash_InvalidRangeValues() {
		Range testRange = new Range(4, 2);
		testRange.hashCode();
	}
	
	@Test
	public void shift_ZeroValueShiftPositiveValidValue() {
		Range testRange = new Range(0, 0);
		Range resultRange = Range.shift(testRange, 1.2);
		Range expectedRange = new Range(1.2, 1.2);
		assertEquals(resultRange, expectedRange);
	}
	
	@Test
	public void shift_ZeroValueShiftNegativeValidValue() {
		Range testRange = new Range(0, 0);
		Range resultRange = Range.shift(testRange, -1.2);
		Range expectedRange = new Range(-1.2, -1.2);
		assertEquals(resultRange, expectedRange);
	}
	
	@Test
	public void shift_NonZeroValueShiftPositiveValidValue() {
		Range testRange = new Range(5, 10);
		Range resultRange = Range.shift(testRange, 6);
		Range expectedRange = new Range(11, 16);
		assertEquals(resultRange, expectedRange);
	}
	
	
	@Test(expected = IllegalArgumentException.class)
	public void shift_Null() {
		Range testRange = null;
		Range resultRange = Range.shift(testRange, 1.2);
	}
	
	@Test(expected = NullPointerException.class)
	public void intersects_NullRangeValidRange() {
		Range testRange = null;
		testRange.intersects(5.1, 6.2);
	}
	
	@Test(expected = NullPointerException.class)
	public void intersects_ValidRangeNullRange() {
		Range testRange1 = new Range(1.4, 8.2);
		Range testRange2 = null;
		testRange1.intersects(testRange2);
	}
	
	@Test
	public void intersects_ValidRangePositiveValidValues() {
		Range testRange = new Range(4, 10);
		assertTrue(testRange.intersects(5, 5));
	}
	
	@Test
	public void intersects_ValidRangePositiveOutOfRangeLeft() {
		Range testRange = new Range(4, 10);
		assertFalse(testRange.intersects(10, 10));
	}
	
	@Test
	public void intersects_ValidRangePositiveOutOfRangeRight() {
		Range testRange = new Range(4, 10);
		assertFalse(testRange.intersects(4, 4));
	}
	
	@Test
	public void intersects_ValidRangeValidRangePositiveOutOfRange() {
		Range testRange = new Range(4, 10);
		assertFalse(testRange.intersects(100, 10));
	}

	@Test
	public void intersects_ValidRangeNegativeValidValues() {
		Range testRange = new Range(-18,-12);
		assertTrue(testRange.intersects(-14, -14));
	}
	
	@Test
	public void intersects_ValidRangeNegativeOutOfRangeRight() {
		Range testRange = new Range(-18,-12);
		assertFalse(testRange.intersects(-12,-12));
	}

	@Test
	public void intersects_ValidRangeNegativeOutOfRangeLeft() {
		Range testRange = new Range(-18,-12);
		assertFalse(testRange.intersects(-18, -18));
	}
	
	@Test(expected = IllegalArgumentException.class)
	public void expand_Null() {
		Range testRange = null;
		Range.expand(testRange, 1, 2);
	}

	
	@After
	public void tearDown() throws Exception {
	}

	@AfterClass
	public static void tearDownAfterClass() throws Exception {
	}
}
```

### Original Test Suite for Test Class `DataUtilities` From Lab 3, Without Modifications for Mutation Coverage

```java
package org.jfree.data;

import static org.junit.Assert.*;

import org.jfree.data.DataUtilities;
import org.jfree.data.DefaultKeyedValues2D;
import org.jfree.data.KeyedValues;
import org.jfree.data.Range;
import org.jfree.data.Values2D;
import org.junit.After;
import org.junit.AfterClass;
import org.junit.Before;
import org.junit.BeforeClass;
import org.junit.Test;
import org.jmock.Mockery;
import org.jmock.Expectations;
import org.jfree.data.DefaultKeyedValue;
import org.jmock.*;

public class DataUtilitiesTest extends DataUtilities {

	@Test(expected = IllegalArgumentException.class)
	public void createNumberArray2d_NullValue() {
		double[][] testArray = null;
		
		DataUtilities.createNumberArray2D(testArray);
	}
	
    @Test
    public void createNumberArray2D_EmptyArray() {
        double[][] testArray = new double[][] {};
        
        assertArrayEquals("The created Number Array is empty.",testArray, DataUtilities.createNumberArray2D(testArray));
    }

    @Test
    public void createNumberArray2D_ValidArray() {
        double[][] testArray = new double[][] {{1.1, 12, 6.77}, {25, 65.4, 22.22}};
        
        assertArrayEquals("The created Number Array contains {{1.1, 12, 6.77}, {25, 65.4, 22.22}}.",testArray, DataUtilities.createNumberArray2D(testArray));
    }

    @Test
    public void createNumberArray2D_ExtremeValue() {
        double[][] testArray = new double [][] {{Double.MAX_VALUE, Double.MAX_VALUE}, {Double.MAX_VALUE, Double.MAX_VALUE}};
        
        assertArrayEquals("The created Number Array contains extreme values."
        		,testArray, DataUtilities.createNumberArray2D(testArray));
    }
    
    @Test
    public void createNumberArray2D_SmallNegativeValue() {
        double[][] testArray = new double [][] {{Double.MIN_VALUE, Double.MIN_VALUE, Double.MIN_VALUE}, 
        	{Double.MIN_VALUE, Double.MIN_VALUE}};
        
        assertArrayEquals("The created Number Array contains very small negative values."
        		,testArray, DataUtilities.createNumberArray2D(testArray));
    }
    
    @Test
    public void createNumberArray2D_NaNValue() {
        double[][] testArray = new double [][] {{Double.NaN, Double.NaN, Double.NaN}, 
        	{Double.NaN, Double.NaN}};
        
        assertArrayEquals("The created Number Array contains NaN values."
        		,testArray, DataUtilities.createNumberArray2D(testArray));
    }
    
    @Test
    public void createNumberArray2D_PositiveInf() {
        double[][] testArray = new double [][] {{Double.POSITIVE_INFINITY, Double.POSITIVE_INFINITY}, 
        	{Double.POSITIVE_INFINITY}};
        
        assertArrayEquals("The created Number Array contains positive infinity values."
        		,testArray, DataUtilities.createNumberArray2D(testArray));
    }
    
    @Test
    public void createNumberArray2D_NegativeInf() {
        double[][] testArray = new double [][] {{Double.NEGATIVE_INFINITY, Double.NEGATIVE_INFINITY, Double.NEGATIVE_INFINITY}, 
        	{Double.NEGATIVE_INFINITY}};
        
        assertArrayEquals("The created Number Array contains negative infinity values."
        		,testArray, DataUtilities.createNumberArray2D(testArray));
    }
    
    //-----------------------------------------------------------------------------------------------------------------------------//
    
    @Test
    public void calculateRowTotal_NullDataValidRow() {
        Mockery mockingContext = new Mockery();
        final Values2D values = mockingContext.mock(Values2D.class);
        mockingContext.checking(new Expectations() {
            {
                one(values).getColumnCount();
                will(returnValue(3));
                one(values).getValue(0, 0);
                will(returnValue(null));
                one(values).getValue(0, 1);
                will(returnValue(null));
                one(values).getValue(0, 2);
                will(returnValue(null));
            }
        });
        double result = DataUtilities.calculateRowTotal(values, 0);
        assertEquals("The calculated total is 0.0",result, 0.0, .000000001d);
    }
    
    @Test
    public void calculateRowTotal_ValidDataValidRow() {
        Mockery mockingContext = new Mockery();
        final Values2D values = mockingContext.mock(Values2D.class);
        mockingContext.checking(new Expectations() {
            {
                one(values).getColumnCount();
                will(returnValue(3));
                one(values).getValue(0, 0);
                will(returnValue(5.67));
                one(values).getValue(0, 1);
                will(returnValue(5.2));
                one(values).getValue(0, 2);
                will(returnValue(1.2));
                one(values).getValue(1, 0);
                will(returnValue(1.7));
            }
        });
        double result = DataUtilities.calculateRowTotal(values, 0);
        assertEquals("The calculated total is 12.07",result, 12.07, .000000001d);
    }
    
    @Test
    public void calculateRowTotal_ValidDataExtremeValuesValidRow() {
        Mockery mockingContext = new Mockery();
        final Values2D values = mockingContext.mock(Values2D.class);
        mockingContext.checking(new Expectations() {
            {
                one(values).getColumnCount();
                will(returnValue(2));
                one(values).getValue(2, 0);
                will(returnValue(Double.MAX_VALUE));
                one(values).getValue(2, 1);
                will(returnValue(Double.MAX_VALUE));
                one(values).getValue(2, 2);
            }
        });
        double result = DataUtilities.calculateRowTotal(values, 2);
        assertEquals("The calculated total is Double.MAX_VALUE+Double.MAX_VALUE",result, (Double.MAX_VALUE+Double.MAX_VALUE), .000000001d);
    }

    @Test
    public void calculateRowTotal_ValidDataNegativeValueValidRow() {
        Mockery mockingContext = new Mockery();
        final Values2D values = mockingContext.mock(Values2D.class);
        mockingContext.checking(new Expectations() {
            {
                one(values).getColumnCount();
                will(returnValue(3));
                one(values).getValue(1, 0);
                will(returnValue(Double.MIN_VALUE));
                one(values).getValue(1, 1);
                will(returnValue(Double.MIN_VALUE));
                one(values).getValue(1, 2);
                will(returnValue(Double.MIN_VALUE));
            }
        });
        double result = DataUtilities.calculateRowTotal(values, 1);
        assertEquals("The calculated total is Double.MIN_VALUE+Double.MIN_VALUE+Double.MIN_VALUE"
        		,result, (Double.MIN_VALUE+Double.MIN_VALUE+Double.MIN_VALUE), .000000001d);
    }
    
    @Test(expected = IndexOutOfBoundsException.class)
    public void calculateRowTotal_ValidDataInvalidRowAboveUpperBoundary() {
    	DefaultKeyedValues2D test = new DefaultKeyedValues2D();
    	test.addValue(1, 0, 0);
        Values2D values = test;
        
        DataUtilities.calculateRowTotal(values, 5);
    }
    

    @Test(expected = IndexOutOfBoundsException.class)
    public void calculateRowTotal_ValidDataInvalidRowBelowLowerBoundary() {
    	DefaultKeyedValues2D test = new DefaultKeyedValues2D();
    	test.addValue(1, 0, 0);
        Values2D values = test;
        
        DataUtilities.calculateRowTotal(values, -2);
    }
    
    //-----------------------------------------------------------------------------------------------------------------------------//
    
    @Test
    public void calculateRowTotal_ValidDataValidRowValidColumns() {
        Mockery mockingContext = new Mockery();
        final Values2D values = mockingContext.mock(Values2D.class);
        mockingContext.checking(new Expectations() {
            {
                one(values).getColumnCount();
                will(returnValue(3));
                one(values).getRowCount();
                will(returnValue(3));
                one(values).getValue(0, 0);
                will(returnValue(4));
                one(values).getValue(1, 1);
                will(returnValue(5));
                one(values).getValue(1, 2);
                will(returnValue(9));
            }
        });
        int[] columns = {1, 2};
        double result = DataUtilities.calculateRowTotal(values, 1, columns);
        assertEquals("The calculated total is 14.0",result, 14.0, .000000001d);
    }
    
    @Test
    public void calculateRowTotal_NullDataValidRowValidColumns() {
        Mockery mockingContext = new Mockery();
        final Values2D values = mockingContext.mock(Values2D.class);
        mockingContext.checking(new Expectations() {
            {
                one(values).getColumnCount();
                will(returnValue(3));
                one(values).getRowCount();
                will(returnValue(3));
                one(values).getValue(0, 0);
                will(returnValue(null));
                one(values).getValue(0, 1);
                will(returnValue(null));
                one(values).getValue(0, 2);
                will(returnValue(null));
            }
        });
        int[] columns = {0, 1, 2};
        double result = DataUtilities.calculateRowTotal(values, 0, columns);
        assertEquals("The calculated total is 0.0",result, 0.0, .000000001d);
    }
    
    @Test
    public void calculateRowTotal_ExtremeDataValidRowValidColumns() {
        Mockery mockingContext = new Mockery();
        final Values2D values = mockingContext.mock(Values2D.class);
        mockingContext.checking(new Expectations() {
            {
                one(values).getColumnCount();
                will(returnValue(3));
                one(values).getRowCount();
                will(returnValue(3));
                one(values).getValue(0, 0);
                will(returnValue(Double.MAX_VALUE));
                one(values).getValue(0, 1);
                will(returnValue(Double.MAX_VALUE));
            }
        });
        int[] columns = {0, 1};
        double result = DataUtilities.calculateRowTotal(values, 0, columns);
        assertEquals("The calculated total is Double.MAX_VALUE+Double.MAX_VALUE",result, Double.MAX_VALUE+Double.MAX_VALUE, .000000001d);
    }
    
    @Test
    public void calculateRowTotal_SmallNegativeDataValidRowValidColumns() {
        Mockery mockingContext = new Mockery();
        final Values2D values = mockingContext.mock(Values2D.class);
        mockingContext.checking(new Expectations() {
            {
                one(values).getColumnCount();
                will(returnValue(3));
                one(values).getRowCount();
                will(returnValue(3));
                one(values).getValue(2, 0);
                will(returnValue(Double.MIN_VALUE));
                one(values).getValue(2, 1);
                will(returnValue(Double.MIN_VALUE));
            }
        });
        int[] columns = {0, 1};
        double result = DataUtilities.calculateRowTotal(values, 2, columns);
        assertEquals("The calculated total is Double.MIN_VALUE+Double.MIN_VALUE",result, 0.0, .000000001d);
    }
    
    
    @Test(expected = IndexOutOfBoundsException.class)
    public void calculateRowTotal_ValidDataValidRowInvalidColumnBelowLowerBoundary() {
    	DefaultKeyedValues2D test = new DefaultKeyedValues2D();
    	int row = 0;
    	int column = 0;
    	test.addValue(1, row, column);
        Values2D values = test;
        
        int[] columns = {-5, 0};
        
        DataUtilities.calculateRowTotal(values, 0, columns);
    }
    
    // @Test(expected = IndexOutOfBoundsException.class)
    // public void calculateRowTotal_ValidDataValidRowInvalidColumnAboveUpperBoundary() {
    // 	DefaultKeyedValues2D test = new DefaultKeyedValues2D();
    // 	int row = 0;
    // 	int column = 0;
    // 	test.addValue(1, row, column);
    //     Values2D values = test;
        
    //     int[] columns = {3, 4};
        
    //     DataUtilities.calculateRowTotal(values, 0, columns);
    // }
    
    @Test(expected = IndexOutOfBoundsException.class)
    public void calculateRowTotal_ValidDataInvalidRowBelowLowerBoundaryValidColumn() {
    	DefaultKeyedValues2D test = new DefaultKeyedValues2D();
    	int row = 0;
    	int column = 0;
    	test.addValue(1, row, column);
        Values2D values = test;
        
        int[] columns = {0};
        
        DataUtilities.calculateRowTotal(values, -1, columns);
    }
    
    @Test(expected = IndexOutOfBoundsException.class)
    public void calculateRowTotal_ValidDataInvalidRowAboveUpperBoundaryValidColumn() {
    	DefaultKeyedValues2D test = new DefaultKeyedValues2D();
    	int row = 0;
    	int column = 0;
    	test.addValue(1, row, column);
        Values2D values = test;
        
        int[] columns = {0};
        
        DataUtilities.calculateRowTotal(values, -1, columns);
    }
    
    
    
    
    //-----------------------------------------------------------------------------------------------------------------------------//
    
    @Test
    public void getCumulativePercentages_ValidValues() {
        Mockery mockingContext = new Mockery();
        final KeyedValues values = mockingContext.mock(KeyedValues.class);
        mockingContext.checking(new Expectations() {
            {
            	allowing(values).getItemCount();
                will(returnValue(2));
                allowing(values).getKey(0);
                will(returnValue(0));
                allowing(values).getValue(0);
                will(returnValue(6.0));
                allowing(values).getKey(1);
                will(returnValue(1));
                allowing(values).getValue(1);
                will(returnValue(4.0));
            }
        });
        KeyedValues result = DataUtilities.getCumulativePercentages(values);
        
        Number[] expectedResults = {(6.0/10.0), 1.0};
        Number [] actualResults = new Number[2];
        for(int i = 0; i < result.getItemCount(); i++) {
        	actualResults[i] = result.getValue(i);
        }
        
        assertArrayEquals("The KeyedValues object contains cumulative percentages 0.6 and 1.0",expectedResults, actualResults);
    }
    
    @Test
    public void getCumulativePercentages_ExtremeValues() {
    	Mockery mockingContext = new Mockery();
        final KeyedValues values = mockingContext.mock(KeyedValues.class);
        mockingContext.checking(new Expectations() {
            {
            	allowing(values).getItemCount();
                will(returnValue(2));
                allowing(values).getKey(0);
                will(returnValue(0));
                allowing(values).getValue(0);
                will(returnValue(Double.MAX_VALUE));
                allowing(values).getKey(1);
                will(returnValue(1));
                allowing(values).getValue(1);
                will(returnValue(Double.MAX_VALUE));
            }
        });
        KeyedValues result = DataUtilities.getCumulativePercentages(values);
        
        Number[] expectedResults = {(Double.MAX_VALUE/(Double.MAX_VALUE+Double.MAX_VALUE)), 
        		((Double.MAX_VALUE+Double.MAX_VALUE)/(Double.MAX_VALUE+Double.MAX_VALUE))};
        Number [] actualResults = new Number[2];
        for(int i = 0; i < result.getItemCount(); i++) {
        	actualResults[i] = result.getValue(i);
        }
        
        assertArrayEquals("The KeyedValues object contains cumulative percentages (Double.MAX_VALUE/(Double.MAX_VALUE+Double.MAX_VALUE) and ((Double.MAX_VALUE+Double.MAX_VALUE)/(Double.MAX_VALUE+Double.MAX_VALUE))"
        		,expectedResults, actualResults);
    }
    
    @Test
    public void getCumulativePercentages_ZeroValues() {
        Mockery mockingContext = new Mockery();
        final KeyedValues values = mockingContext.mock(KeyedValues.class);
        mockingContext.checking(new Expectations() {
            {
            	allowing(values).getItemCount();
                will(returnValue(2));
                allowing(values).getKey(0);
                will(returnValue(0));
                allowing(values).getValue(0);
                will(returnValue(0));
                allowing(values).getKey(1);
                will(returnValue(1));
                allowing(values).getValue(1);
                will(returnValue(0));
            }
        });
        KeyedValues result = DataUtilities.getCumulativePercentages(values);

        Number[] expectedResults = {Double.NaN, Double.NaN};
        Number [] actualResults = {0.0, 0.0};
        for(int i = 0; i < result.getItemCount(); i++) {
        	actualResults[i] = result.getValue(i);
        }
        
        assertArrayEquals("The KeyedValues object contains cumulative percentages Double.NaN and Double.NaN",expectedResults, actualResults);
    }
    
    @Test
    public void getCumulativePercentages_WithNullValues() {
    	Mockery mockingContext = new Mockery();
        final KeyedValues values = mockingContext.mock(KeyedValues.class);
        mockingContext.checking(new Expectations() {
            {
            	allowing(values).getItemCount();
                will(returnValue(4));
                allowing(values).getKey(0);
                will(returnValue(0));
                allowing(values).getValue(0);
                will(returnValue(null));
                allowing(values).getKey(1);
                will(returnValue(1));
                allowing(values).getValue(1);
                will(returnValue(4.5));
                allowing(values).getKey(2);
                will(returnValue(2));
                allowing(values).getValue(2);
                will(returnValue(null));
                allowing(values).getKey(3);
                will(returnValue(3));
                allowing(values).getValue(3);
                will(returnValue(3.5));
            }
        });
        KeyedValues result = DataUtilities.getCumulativePercentages(values);
        
        Number[] expectedResults = {0.0, (4.5/8.0), (4.5/8.0), 1.0};
        Number [] actualResults = new Number[4];
        for(int i = 0; i < result.getItemCount(); i++) {
        	actualResults[i] = result.getValue(i);
        }
        
        assertArrayEquals("The KeyedValues object contains cumulative percentages 0.5625 and 0.5625",expectedResults, actualResults);
    }
    
    
    @Test(expected = IllegalArgumentException.class)
    public void getCumulativePercentages_NullData() {
    	KeyedValues values = null;
    	DataUtilities.getCumulativePercentages(values);
    }
    
    
    //-----------------------------------------------------------------------------------------------------------------------------//

    private DataUtilities exampleDataUtilities;
    @BeforeClass public static void setUpBeforeClass() throws Exception {
    }
   

    //Normal Input with non-null values
    @Test
    public void createNumberArray_NonNullDataInput() {
        //Sample Input with Non Null Data Input
        double [] data = { 13.4, 12.00004, 0.001, 1235.0, 234};
        Number [] result = DataUtilities.createNumberArray(data);
        Number [] expected = { 13.4, 12.00004, 0.001, 1235.0, 234.0};
        
        assertArrayEquals("The data input should include non-null double values", expected, result);
    }

    //Too Large value. Checked for Conversion. 
    @Test
    public void createNumberArray_ExtremeDataInput() {
        //Sample Input with Extreme (large/small) doubles
        double [] data = { Double.MAX_VALUE, Double.MAX_VALUE};
        Number [] result = DataUtilities.createNumberArray(data);
        Number [] expected = { Double.MAX_VALUE, Double.MAX_VALUE};
        
        assertArrayEquals("Extreme values should be allowed (large doubles)", expected, result);
    }
    
    //Too small value. Checked for Conversion. 
    @Test
    public void createNumberArray_SmallNegativeDataInput() {
        //Sample Input with Extreme (large/small) doubles
        double [] data = { Double.MIN_VALUE, Double.MIN_VALUE};
        Number [] result = DataUtilities.createNumberArray(data);
        Number [] expected = { Double.MIN_VALUE, Double.MIN_VALUE};
        
        assertArrayEquals("Extreme values should be allowed (small doubles)", expected, result);
    }
    
    //Invalid Array is used as an Argument. 
    @Test
     public void createNumberArray_EmptyArray() {
         //Sample Input with Null. Expect a null return
         double [] data = {};
         Number [] result = DataUtilities.createNumberArray(data);
         assertNotNull("Checking that createNumberArray can throw exception when a null array is sent",  result);
     }
    
    //Array is set to null
	@Test(expected = IllegalArgumentException.class)
	public void createNumberArray_NullValue() {
		double[] data = null;
		
		DataUtilities.createNumberArray(data);
	}
	
    @Test
    public void createNumberArray_NaNValue() {
        //Sample Input with NaN Data Input
        double [] data = {Double.NaN, Double.NaN, Double.NaN};
        Number [] result = DataUtilities.createNumberArray(data);
        Number [] expected = {Double.NaN, Double.NaN, Double.NaN};
        
        assertArrayEquals("The data input should include NaN double values", expected, result);
    }
    
    @Test
    public void createNumberArray_PositiveInf() {
        //Sample Input with Positive Infinity Data Input
        double [] data = { Double.POSITIVE_INFINITY, Double.POSITIVE_INFINITY};
        Number [] result = DataUtilities.createNumberArray(data);
        Number [] expected = { Double.POSITIVE_INFINITY, Double.POSITIVE_INFINITY};
        
        assertArrayEquals("The data input should include Positive Infinity double values", expected, result);
    }
    
    @Test
    public void createNumberArray_NegativeInf() {
        //Sample Input with Negative Infinity Data Input
        double [] data = { Double.NEGATIVE_INFINITY, Double.NEGATIVE_INFINITY, Double.NEGATIVE_INFINITY};
        Number [] result = DataUtilities.createNumberArray(data);
        Number [] expected = { Double.NEGATIVE_INFINITY, Double.NEGATIVE_INFINITY, Double.NEGATIVE_INFINITY};
        
        assertArrayEquals("The data input should include Negative Infinity double values", expected, result);
    }

    
    //-----------------------------------------------------------------------------------------------------------------------------//
	
    @Test
    public void calculateColumnTotal_NullDataValidColumn() {
        Mockery mockingContext = new Mockery();
        final Values2D values = mockingContext.mock(Values2D.class);
        mockingContext.checking(new Expectations() {
            {
                one(values).getRowCount();
                will(returnValue(3));
                one(values).getValue(0, 0);
                will(returnValue(null));
                one(values).getValue(1, 0);
                will(returnValue(null));
                one(values).getValue(2, 0);
                will(returnValue(null));
            }
        });
        double result = DataUtilities.calculateColumnTotal(values, 0);
        assertEquals("The calculated total is 0.0",result, 0.0, .000000001d);
    }
    
    @Test
    public void calculateColumnTotal_ValidDataValidColumn() {
        Mockery mockingContext = new Mockery();
        final Values2D values = mockingContext.mock(Values2D.class);
        mockingContext.checking(new Expectations() {
            {
                one(values).getRowCount();
                will(returnValue(3));
                one(values).getValue(0, 0);
                will(returnValue(5.67));
                one(values).getValue(1, 0);
                will(returnValue(5.2));
                one(values).getValue(2, 0);
                will(returnValue(1.2));
                one(values).getValue(0, 1);
                will(returnValue(1.7));
            }
        });
        double result = DataUtilities.calculateColumnTotal(values, 0);
        assertEquals("The calculated total is 12.07",result, 12.07, .000000001d);
    }
    
    @Test
    public void calculateColumnTotal_ValidDataExtremeValuesValidColumn() {
        Mockery mockingContext = new Mockery();
        final Values2D values = mockingContext.mock(Values2D.class);
        mockingContext.checking(new Expectations() {
            {
                one(values).getRowCount();
                will(returnValue(2));
                one(values).getValue(0, 2);
                will(returnValue(Double.MAX_VALUE));
                one(values).getValue(1, 2);
                will(returnValue(Double.MAX_VALUE));
                one(values).getValue(2, 2);
            }
        });
        double result = DataUtilities.calculateColumnTotal(values, 2);
        assertEquals("The calculated total is Double.MAX_VALUE+Double.MAX_VALUE",result, (Double.MAX_VALUE+Double.MAX_VALUE), .000000001d);
    }

    @Test
    public void calculateColumnTotal_ValidDataNegativeValueValidColumn() {
        Mockery mockingContext = new Mockery();
        final Values2D values = mockingContext.mock(Values2D.class);
        mockingContext.checking(new Expectations() {
            {
                one(values).getRowCount();
                will(returnValue(3));
                one(values).getValue(0, 1);
                will(returnValue(Double.MIN_VALUE));
                one(values).getValue(1, 1);
                will(returnValue(Double.MIN_VALUE));
                one(values).getValue(2, 1);
                will(returnValue(Double.MIN_VALUE));
            }
        });
        double result = DataUtilities.calculateColumnTotal(values, 1);
        assertEquals("The calculated total is Double.MIN_VALUE+Double.MIN_VALUE+Double.MIN_VALUE"
        		,result, (Double.MIN_VALUE+Double.MIN_VALUE+Double.MIN_VALUE), .000000001d);
    }
    
    @Test(expected = IndexOutOfBoundsException.class)
    public void calculateColumnTotal_ValidDataInvalidColumnAboveUpperBoundary() {
    	DefaultKeyedValues2D test = new DefaultKeyedValues2D();
    	test.addValue(1, 0, 0);
        Values2D values = test;
        
        DataUtilities.calculateColumnTotal(values, 5);
    }
    

    @Test(expected = IndexOutOfBoundsException.class)
    public void calculateColumnTotal_ValidDataInvalidColumnBelowLowerBoundary() {
    	DefaultKeyedValues2D test = new DefaultKeyedValues2D();
    	test.addValue(1, 0, 0);
        Values2D values = test;
        
        DataUtilities.calculateColumnTotal(values, -2);
    }
	
	 //-----------------------------------------------------------------------------------------------------------------------------//
    
    Mockery context;

    //Checking whether calculateColumnTotal works as expected with non-null values
    @Before
    public void setUp_NonNullAndValidColumn() throws Exception { 
        context = new Mockery();
    }
    
    @Test
    public void calculateColumnTotal_NonNullAndValidColumn(){
        final org.jfree.data.Values2D values = context.mock(org.jfree.data.Values2D.class);

        int dataRowCount = 3;
        int column = 2;

        context.checking(new Expectations() {{
            one(values).getRowCount();
            will(returnValue(dataRowCount));
            one(values).getValue(0,column);
            will(returnValue(2.4));
            one(values).getValue(1,column);
            will(returnValue(7.2));
            one(values).getValue(2,column);
            will(returnValue(9.4));
        }});
        
        int [] validRows = {0,1,2};

        double result = DataUtilities.calculateColumnTotal(values, column, validRows);
        assertEquals("Method cannot support a valid Values2D object, column and valid rows", result, 19.0, .000000001d);
    }

    @After
    public void tearDown_NonNullAndValidColumn() throws Exception {
        context = null;
    }

    //Test a column value not allowed by Value 2D Array. ie < 0
    @Before
     public void setUp_TooSmallColumn() throws Exception { 
         context = new Mockery();
     }
     
     @Test(expected = IndexOutOfBoundsException.class)
     public void calculateColumnTotal_TooSmallColumn(){
         DefaultKeyedValues2D test = new DefaultKeyedValues2D();
         test.addValue(1, 0, 0);
         org.jfree.data.Values2D values = (org.jfree.data.Values2D) test;
 
         int [] validRows = {0};
         DataUtilities.calculateColumnTotal((org.jfree.data.Values2D) values, -1, validRows);
     }

     @After
     public void tearDown_TooSmallColumn() throws Exception {
         context = null;
     }
 
    //Testing column that is too large. Exception should be thrown. 
    @Test(expected = IndexOutOfBoundsException.class)
    public void calculateColumnTotal_TooLargeColumn(){
        DefaultKeyedValues2D test = new DefaultKeyedValues2D();
        test.addValue(1, 0, 0);
        org.jfree.data.Values2D values = (org.jfree.data.Values2D) test;

        int [] validRows = {0};
        DataUtilities.calculateColumnTotal((org.jfree.data.Values2D) values, 5, validRows);
    }

    //Upper Boundary Valid Rows check. Viewing
    @Before
    public void setUp_ValidRowsExceeded() throws Exception { 
        context = new Mockery();
    }
    
    // @Test(expected = IndexOutOfBoundsException.class)
    // public void calculateColumnTotal_ValidRowsExceeded(){
    //     final org.jfree.data.Values2D values = context.mock(org.jfree.data.Values2D.class);

    //     int dataRowCount = 3;
    //     int column = 2;

    //     context.checking(new Expectations() {{
    //         one(values).getRowCount();
    //         will(returnValue(dataRowCount));
    //     }});
        
    //     int [] validRows = {4};
    //     DataUtilities.calculateColumnTotal(values, 3, validRows);
    // }

    @After
    public void tearDown_ValidRowsExceeded() throws Exception {
        context = null;
    }

    //Testing whether Invalid Row (<0) is accepted. Exception thrown by methods means test passes
    @Test
    public void calculateColumnTotal_RowsBelowZero(){
        DefaultKeyedValues2D test = new DefaultKeyedValues2D();
        test.addValue(1, 0, 0);
        org.jfree.data.Values2D values = (org.jfree.data.Values2D) test;

        int [] validRows = {-1};

        try{
           double value = DataUtilities.calculateColumnTotal((org.jfree.data.Values2D) values, 0, validRows);
           assertNull("Calculation should throw exception to out of bounds value (<0) for Rows",value);
        } catch (Exception e){

        }         
    }
 
	 //-----------------------------------------------------------------------------------------------------------------------------//
    
    @Test
    public void Equal_ValidPositiveValuesArrays() {
    	double[][] array1 = {{15.0, 12.67}, {5.6, 7.8}};
    	double[][] array2 = {{15.0, 12.67}, {5.6, 7.8}};
    	
    	assertTrue("Equal method is expected to return true with positive values.", DataUtilities.equal(array1, array2));
    }
    
    @Test
    public void Equal_ValidNegativeValuesArrays() {
    	double[][] array1 = {{-15.0, -12.67}, {-5.6, -7.8}};
    	double[][] array2 = {{-15.0, -12.67}, {-5.6, -7.8}};
    	
    	assertTrue("Equal method is expected to return true with negative values.", DataUtilities.equal(array1, array2));
    }
    
    @Test
    public void Equal_NullArrays() {
    	double[][] array1 = null;
    	double[][] array2 = null;
    	
    	assertTrue("Equal method is expected to return true with null values.", DataUtilities.equal(array1, array2));
    }
    
    @Test
    public void Equal_ExtemeValueArrays() {
    	double[][] array1 = {{Double.MAX_VALUE, 12.67}, {Double.MAX_VALUE, Double.MAX_VALUE}};
    	double[][] array2 = {{Double.MAX_VALUE, 12.67}, {Double.MAX_VALUE, Double.MAX_VALUE}};
    	
    	assertTrue("Equal method is expected to return true with extreme values.", DataUtilities.equal(array1, array2));
    }
    
    @Test
    public void Equal_SmallNegativeValueArrays() {
    	double[][] array1 = {{Double.MIN_VALUE, Double.MIN_VALUE}, {Double.MIN_VALUE, Double.MIN_VALUE}};
    	double[][] array2 = {{Double.MIN_VALUE, Double.MIN_VALUE}, {Double.MIN_VALUE, Double.MIN_VALUE}};
    	
    	assertTrue("Equal method is expected to return true with very small negative values.", DataUtilities.equal(array1, array2));
    }
    
    @Test
    public void Equal_EmptyArrays() {
    	double[][] array1 = {{}, {}};
    	double[][] array2 = {{}, {}};
    	
    	assertTrue("Equal method is expected to return true with empty arrays.", DataUtilities.equal(array1, array2));
    }
    
    @Test
    public void Equal_DifferentValueArrays() {
    	double[][] array1 = {{12.55, 1.0}, {13.4, 7.8}};
    	double[][] array2 = {{12.67, 1.1}, {16.4, 9.8}};
    	
    	assertFalse("Equal method is expected to return false with arrays containing different values.", DataUtilities.equal(array1, array2));
    }
    
    @Test
    public void Equal_NaNValueArrays() {
    	double[][] array1 = {{Double.NaN, Double.NaN}, {Double.NaN, Double.NaN}};
    	double[][] array2 = {{Double.NaN, Double.NaN}, {Double.NaN, Double.NaN}};
    	
    	assertTrue("Equal method is expected to return true with NaN values.", DataUtilities.equal(array1, array2));
    }
    
    @Test
    public void Equal_PositiveInfValueArrays() {
    	double[][] array1 = {{Double.POSITIVE_INFINITY, Double.POSITIVE_INFINITY}};
    	double[][] array2 = {{Double.POSITIVE_INFINITY, Double.POSITIVE_INFINITY}};
    	
    	assertTrue("Equal method is expected to return true with positive infinity values.", DataUtilities.equal(array1, array2));
    }
    
    @Test
    public void Equal_NegativeInfValueArrays() {
    	double[][] array1 = {{Double.NEGATIVE_INFINITY, Double.NEGATIVE_INFINITY}};
    	double[][] array2 = {{Double.NEGATIVE_INFINITY, Double.NEGATIVE_INFINITY}};
    	
    	assertTrue("Equal method is expected to return true with negative infinity values.", DataUtilities.equal(array1, array2));
    }
    
	 //-----------------------------------------------------------------------------------------------------------------------------//
    
    @Test
    public void Clone_ValidPositiveValuesArrayData() {
    	double [][] expected = {{15.0, 12.67}, {5.6, 7.8}};
    	double [][] result = DataUtilities.clone(expected);
    	
    	assertArrayEquals("Clone method returns double 2D array containing {15.0, 12.67}, {5.6, 7.8}.", result, expected);
    }
    
    @Test
    public void Clone_ValidNegativeValuesArrayData() {
    	double [][] expected = {{-15.0, -12.67}, {-5.6, -7.8}};
    	double [][] result = DataUtilities.clone(expected);
    	
    	assertArrayEquals("Clone method returns double 2D array containing {-15.0, -12.67}, {-5.6, -7.8}.", result, expected);
    }
    
    @Test(expected = IllegalArgumentException.class)
    public void Clone_NullArrayData() {
    	double [][] expected = null;
    	DataUtilities.clone(expected);
    }
    
    @Test
    public void Clone_EmptyArrayData() {
    	double [][] expected = {{}};
    	double [][] result = DataUtilities.clone(expected);
    	
    	assertArrayEquals("Clone method returns double 2D array that is empty.", result, expected);
    }
    
    @Test
    public void Clone_ExtremeValuesArrayData() {
    	double [][] expected = {{Double.MAX_VALUE, 12.67}, {Double.MAX_VALUE, Double.MAX_VALUE}};
    	double [][] result = DataUtilities.clone(expected);
    	
    	assertArrayEquals("Clone method returns double 2D array containing {Double.MAX_VALUE, 12.67}, {Double.MAX_VALUE, Double.MAX_VALUE}.", result, expected);
    }
    
    @Test
    public void Clone_SmallNegativeValuesArrayData() {
    	double [][] expected = {{Double.MIN_VALUE, Double.MIN_VALUE}, {Double.MIN_VALUE, Double.MIN_VALUE}};
    	double [][] result = DataUtilities.clone(expected);
    	
    	assertArrayEquals("Clone method returns double 2D array containing {Double.MIN_VALUE, Double.MIN_VALUE}, {Double.MIN_VALUE, Double.MIN_VALUE}.", result, expected);
    }
    
    @Test
    public void Clone_NaNArrayData() {
    	double [][] expected = {{Double.NaN, Double.NaN}};
    	double [][] result = DataUtilities.clone(expected);
    	
    	assertArrayEquals("Clone method returns double 2D array containing {Double.POSITIVE_INFINITY, Double.POSITIVE_INFINITY}.", result, expected);
    }
    
    @Test
    public void Clone_PositiveInfArrayData() {
    	double [][] expected = {{Double.POSITIVE_INFINITY, Double.POSITIVE_INFINITY}};
    	double [][] result = DataUtilities.clone(expected);
    	
    	assertArrayEquals("Clone method returns double 2D array containing {Double.POSITIVE_INFINITY, Double.POSITIVE_INFINITY}.", result, expected);
    }
    
    @Test
    public void Clone_NegativeInfArrayData() {
    	double [][] expected = {{Double.NEGATIVE_INFINITY, Double.NEGATIVE_INFINITY}};
    	double [][] result = DataUtilities.clone(expected);
    	
    	assertArrayEquals("Clone method returns double 2D array containing {Double.NEGATIVE_INFINITY, Double.NEGATIVE_INFINITY}.", result, expected);
    }
    
    //-----------------------------------------------------------------------------------------------------------------------------//

    @After
    public void tearDown() throws Exception {
    }
}
```

### Updated Test Suite for Test Class `DataUtilities` From Lab 4, With Modifications for Mutation Coverage

```java
package org.jfree.data;

import static org.junit.Assert.*;

import java.util.Arrays;

import org.jfree.data.DataUtilities;
import org.jfree.data.DefaultKeyedValues2D;
import org.jfree.data.KeyedValues;
import org.jfree.data.Range;
import org.jfree.data.Values2D;
import org.junit.After;
import org.junit.AfterClass;
import org.junit.Before;
import org.junit.BeforeClass;
import org.junit.Test;
import org.jmock.Mockery;
import org.jmock.Expectations;
import org.jfree.data.DefaultKeyedValue;
import org.jmock.*;

public class DataUtilitiesTest extends DataUtilities {

	@Test(expected = IllegalArgumentException.class)
	public void createNumberArray2d_NullValue() {
		double[][] testArray = null;
		
		DataUtilities.createNumberArray2D(testArray);
	}
	
    @Test()
    public void createNumberArray2D_EmptyArray() {
        double[][] testArray = new double[][] {};
        
        assertArrayEquals("The created Number Array is empty.",testArray, DataUtilities.createNumberArray2D(testArray));
    }

    @Test
    public void createNumberArray2D_ValidArray() {
        double[][] testArray = new double[][] {{1.1, 12, 6.77}, {25, 65.4, 22.22}};
        
        assertArrayEquals("The created Number Array contains {{1.1, 12, 6.77}, {25, 65.4, 22.22}}.",testArray, DataUtilities.createNumberArray2D(testArray));
    }

    @Test
    public void createNumberArray2D_ExtremeValue() {
        double[][] testArray = new double [][] {{Double.MAX_VALUE, Double.MAX_VALUE}, {Double.MAX_VALUE, Double.MAX_VALUE}};
        
        assertArrayEquals("The created Number Array contains extreme values."
        		,testArray, DataUtilities.createNumberArray2D(testArray));
    }
    
    @Test
    public void createNumberArray2D_SmallNegativeValue() {
        double[][] testArray = new double [][] {{Double.MIN_VALUE, Double.MIN_VALUE, Double.MIN_VALUE}, 
        	{Double.MIN_VALUE, Double.MIN_VALUE}};
        
        assertArrayEquals("The created Number Array contains very small negative values."
        		,testArray, DataUtilities.createNumberArray2D(testArray));
    }
    
    @Test
    public void createNumberArray2D_NaNValue() {
        double[][] testArray = new double [][] {{Double.NaN, Double.NaN, Double.NaN}, 
        	{Double.NaN, Double.NaN}};
        
        assertArrayEquals("The created Number Array contains NaN values."
        		,testArray, DataUtilities.createNumberArray2D(testArray));
    }
    
    @Test
    public void createNumberArray2D_PositiveInf() {
        double[][] testArray = new double [][] {{Double.POSITIVE_INFINITY, Double.POSITIVE_INFINITY}, 
        	{Double.POSITIVE_INFINITY}};
        
        assertArrayEquals("The created Number Array contains positive infinity values."
        		,testArray, DataUtilities.createNumberArray2D(testArray));
    }
    
    @Test
    public void createNumberArray2D_NegativeInf() {
        double[][] testArray = new double [][] {{Double.NEGATIVE_INFINITY, Double.NEGATIVE_INFINITY, Double.NEGATIVE_INFINITY}, 
        	{Double.NEGATIVE_INFINITY}};
        
        assertArrayEquals("The created Number Array contains negative infinity values."
        		,testArray, DataUtilities.createNumberArray2D(testArray));
    }
    
    //-----------------------------------------------------------------------------------------------------------------------------//
    
    @Test
    public void calculateRowTotal_NullDataValidRow() {
        Mockery mockingContext = new Mockery();
        final Values2D values = mockingContext.mock(Values2D.class);
        mockingContext.checking(new Expectations() {
            {
                one(values).getColumnCount();
                will(returnValue(3));
                one(values).getValue(0, 0);
                will(returnValue(null));
                one(values).getValue(0, 1);
                will(returnValue(null));
                one(values).getValue(0, 2);
                will(returnValue(null));
            }
        });
        double result = DataUtilities.calculateRowTotal(values, 0);
        assertEquals("The calculated total is 0.0",result, 0.0, .000000001d);
    }
    
    @Test
    public void calculateRowTotal_ValidDataValidRow() {
        Mockery mockingContext = new Mockery();
        final Values2D values = mockingContext.mock(Values2D.class);
        mockingContext.checking(new Expectations() {
            {
                one(values).getColumnCount();
                will(returnValue(3));
                one(values).getValue(0, 0);
                will(returnValue(5.67));
                one(values).getValue(0, 1);
                will(returnValue(5.2));
                one(values).getValue(0, 2);
                will(returnValue(1.2));
                one(values).getValue(1, 0);
                will(returnValue(1.7));
            }
        });
        double result = DataUtilities.calculateRowTotal(values, 0);
        assertEquals("The calculated total is 12.07",result, 12.07, .000000001d);
    }
    
    @Test
    public void calculateRowTotal_ValidDataExtremeValuesValidRow() {
        Mockery mockingContext = new Mockery();
        final Values2D values = mockingContext.mock(Values2D.class);
        mockingContext.checking(new Expectations() {
            {
                one(values).getColumnCount();
                will(returnValue(2));
                one(values).getValue(2, 0);
                will(returnValue(Double.MAX_VALUE));
                one(values).getValue(2, 1);
                will(returnValue(Double.MAX_VALUE));
                one(values).getValue(2, 2);
            }
        });
        double result = DataUtilities.calculateRowTotal(values, 2);
        assertEquals("The calculated total is Double.MAX_VALUE+Double.MAX_VALUE",result, (Double.MAX_VALUE+Double.MAX_VALUE), .000000001d);
    }

    @Test
    public void calculateRowTotal_ValidDataNegativeValueValidRow() {
        Mockery mockingContext = new Mockery();
        final Values2D values = mockingContext.mock(Values2D.class);
        mockingContext.checking(new Expectations() {
            {
                one(values).getColumnCount();
                will(returnValue(3));
                one(values).getValue(1, 0);
                will(returnValue(Double.MIN_VALUE));
                one(values).getValue(1, 1);
                will(returnValue(Double.MIN_VALUE));
                one(values).getValue(1, 2);
                will(returnValue(Double.MIN_VALUE));
            }
        });
        double result = DataUtilities.calculateRowTotal(values, 1);
        assertEquals("The calculated total is Double.MIN_VALUE+Double.MIN_VALUE+Double.MIN_VALUE"
        		,result, (Double.MIN_VALUE+Double.MIN_VALUE+Double.MIN_VALUE), .000000001d);
    }
    
    @Test(expected = IndexOutOfBoundsException.class)
    public void calculateRowTotal_ValidDataInvalidRowAboveUpperBoundary() {
    	DefaultKeyedValues2D test = new DefaultKeyedValues2D();
    	test.addValue(1, 0, 0);
        Values2D values = test;
        
        DataUtilities.calculateRowTotal(values, 5);
    }
    

    @Test(expected = IndexOutOfBoundsException.class)
    public void calculateRowTotal_ValidDataInvalidRowBelowLowerBoundary() {
    	DefaultKeyedValues2D test = new DefaultKeyedValues2D();
    	test.addValue(1, 0, 0);
        Values2D values = test;
        
        DataUtilities.calculateRowTotal(values, -2);
    }
    
    //-----------------------------------------------------------------------------------------------------------------------------//
    
    @Test
    public void calculateRowTotal_ValidDataValidRowValidColumns() {
        Mockery mockingContext = new Mockery();
        final Values2D values = mockingContext.mock(Values2D.class);
        mockingContext.checking(new Expectations() {
            {
                one(values).getColumnCount();
                will(returnValue(3));
                one(values).getRowCount();
                will(returnValue(3));
                one(values).getValue(0, 0);
                will(returnValue(4));
                one(values).getValue(1, 1);
                will(returnValue(5));
                one(values).getValue(1, 2);
                will(returnValue(9));
            }
        });
        int[] columns = {1, 2};
        double result = DataUtilities.calculateRowTotal(values, 1, columns);
        assertEquals("The calculated total is 14.0",result, 14.0, .000000001d);
    }
    
    @Test
    public void calculateRowTotal_NullDataValidRowValidColumns() {
        Mockery mockingContext = new Mockery();
        final Values2D values = mockingContext.mock(Values2D.class);
        mockingContext.checking(new Expectations() {
            {
                one(values).getColumnCount();
                will(returnValue(3));
                one(values).getRowCount();
                will(returnValue(3));
                one(values).getValue(0, 0);
                will(returnValue(null));
                one(values).getValue(0, 1);
                will(returnValue(null));
                one(values).getValue(0, 2);
                will(returnValue(null));
            }
        });
        int[] columns = {0, 1, 2};
        double result = DataUtilities.calculateRowTotal(values, 0, columns);
        assertEquals("The calculated total is 0.0",result, 0.0, .000000001d);
    }
    
    @Test
    public void calculateRowTotal_ExtremeDataValidRowValidColumns() {
        Mockery mockingContext = new Mockery();
        final Values2D values = mockingContext.mock(Values2D.class);
        mockingContext.checking(new Expectations() {
            {
                one(values).getColumnCount();
                will(returnValue(3));
                one(values).getRowCount();
                will(returnValue(3));
                one(values).getValue(0, 0);
                will(returnValue(Double.MAX_VALUE));
                one(values).getValue(0, 1);
                will(returnValue(Double.MAX_VALUE));
            }
        });
        int[] columns = {0, 1};
        double result = DataUtilities.calculateRowTotal(values, 0, columns);
        assertEquals("The calculated total is Double.MAX_VALUE+Double.MAX_VALUE",result, Double.MAX_VALUE+Double.MAX_VALUE, .000000001d);
    }
    
    @Test
    public void calculateRowTotal_SmallNegativeDataValidRowValidColumns() {
        Mockery mockingContext = new Mockery();
        final Values2D values = mockingContext.mock(Values2D.class);
        mockingContext.checking(new Expectations() {
            {
                one(values).getColumnCount();
                will(returnValue(3));
                one(values).getRowCount();
                will(returnValue(3));
                one(values).getValue(2, 0);
                will(returnValue(Double.MIN_VALUE));
                one(values).getValue(2, 1);
                will(returnValue(Double.MIN_VALUE));
            }
        });
        int[] columns = {0, 1};
        double result = DataUtilities.calculateRowTotal(values, 2, columns);
        assertEquals("The calculated total is Double.MIN_VALUE+Double.MIN_VALUE",result, 0.0, .000000001d);
    }
    
    
    @Test(expected = IndexOutOfBoundsException.class)
    public void calculateRowTotal_ValidDataValidRowInvalidColumnBelowLowerBoundary() {
    	DefaultKeyedValues2D test = new DefaultKeyedValues2D();
    	int row = 0;
    	int column = 0;
    	test.addValue(1, row, column);
        Values2D values = test;
        
        int[] columns = {-5, 0};
        
        DataUtilities.calculateRowTotal(values, 0, columns);
    }
    
    // @Test(expected = IndexOutOfBoundsException.class)
    // public void calculateRowTotal_ValidDataValidRowInvalidColumnAboveUpperBoundary() {
    // 	DefaultKeyedValues2D test = new DefaultKeyedValues2D();
    // 	int row = 0;
    // 	int column = 0;
    // 	test.addValue(1, row, column);
    //     Values2D values = test;
        
    //     int[] columns = {3, 4};
        
    //     DataUtilities.calculateRowTotal(values, 0, columns);
    // }
    
    @Test(expected = IndexOutOfBoundsException.class)
    public void calculateRowTotal_ValidDataInvalidRowBelowLowerBoundaryValidColumn() {
    	DefaultKeyedValues2D test = new DefaultKeyedValues2D();
    	int row = 0;
    	int column = 0;
    	test.addValue(1, row, column);
        Values2D values = test;
        
        int[] columns = {0};
        
        DataUtilities.calculateRowTotal(values, -1, columns);
    }
    
    @Test(expected = IndexOutOfBoundsException.class)
    public void calculateRowTotal_ValidDataInvalidRowAboveUpperBoundaryValidColumn() {
    	DefaultKeyedValues2D test = new DefaultKeyedValues2D();
    	int row = 0;
    	int column = 0;
    	test.addValue(1, row, column);
        Values2D values = test;
        
        int[] columns = {0};
        
        DataUtilities.calculateRowTotal(values, -1, columns);
    }

    @Test(expected = IllegalArgumentException.class)
    public void calculateRowTotal_NullDataArgument(){
	double result = DataUtilities.calculateRowTotal(null, 0);
    }
    
    
    
    
    //-----------------------------------------------------------------------------------------------------------------------------//
    
    @Test
    public void getCumulativePercentages_ValidValues() {
        Mockery mockingContext = new Mockery();
        final KeyedValues values = mockingContext.mock(KeyedValues.class);
        mockingContext.checking(new Expectations() {
            {
            	allowing(values).getItemCount();
                will(returnValue(2));
                allowing(values).getKey(0);
                will(returnValue(0));
                allowing(values).getValue(0);
                will(returnValue(6.0));
                allowing(values).getKey(1);
                will(returnValue(1));
                allowing(values).getValue(1);
                will(returnValue(4.0));
            }
        });
        KeyedValues result = DataUtilities.getCumulativePercentages(values);
        
        Number[] expectedResults = {(6.0/10.0), 1.0};
        Number [] actualResults = new Number[2];
        for(int i = 0; i < result.getItemCount(); i++) {
        	actualResults[i] = result.getValue(i);
        }
        
        assertArrayEquals("The KeyedValues object contains cumulative percentages 0.6 and 1.0",expectedResults, actualResults);
    }
    
    @Test
    public void getCumulativePercentages_ExtremeValues() {
    	Mockery mockingContext = new Mockery();
        final KeyedValues values = mockingContext.mock(KeyedValues.class);
        mockingContext.checking(new Expectations() {
            {
            	allowing(values).getItemCount();
                will(returnValue(2));
                allowing(values).getKey(0);
                will(returnValue(0));
                allowing(values).getValue(0);
                will(returnValue(Double.MAX_VALUE));
                allowing(values).getKey(1);
                will(returnValue(1));
                allowing(values).getValue(1);
                will(returnValue(Double.MAX_VALUE));
            }
        });
        KeyedValues result = DataUtilities.getCumulativePercentages(values);
        
        Number[] expectedResults = {(Double.MAX_VALUE/(Double.MAX_VALUE+Double.MAX_VALUE)), 
        		((Double.MAX_VALUE+Double.MAX_VALUE)/(Double.MAX_VALUE+Double.MAX_VALUE))};
        Number [] actualResults = new Number[2];
        for(int i = 0; i < result.getItemCount(); i++) {
        	actualResults[i] = result.getValue(i);
        }
        
        assertArrayEquals("The KeyedValues object contains cumulative percentages (Double.MAX_VALUE/(Double.MAX_VALUE+Double.MAX_VALUE) and ((Double.MAX_VALUE+Double.MAX_VALUE)/(Double.MAX_VALUE+Double.MAX_VALUE))"
        		,expectedResults, actualResults);
    }
    
    @Test
    public void getCumulativePercentages_ZeroValues() {
        Mockery mockingContext = new Mockery();
        final KeyedValues values = mockingContext.mock(KeyedValues.class);
        mockingContext.checking(new Expectations() {
            {
            	allowing(values).getItemCount();
                will(returnValue(2));
                allowing(values).getKey(0);
                will(returnValue(0));
                allowing(values).getValue(0);
                will(returnValue(0));
                allowing(values).getKey(1);
                will(returnValue(1));
                allowing(values).getValue(1);
                will(returnValue(0));
            }
        });
        KeyedValues result = DataUtilities.getCumulativePercentages(values);

        Number[] expectedResults = {Double.NaN, Double.NaN};
        Number [] actualResults = {0.0, 0.0};
        for(int i = 0; i < result.getItemCount(); i++) {
        	actualResults[i] = result.getValue(i);
        }
        
        assertArrayEquals("The KeyedValues object contains cumulative percentages Double.NaN and Double.NaN",expectedResults, actualResults);
    }
    
    @Test
    public void getCumulativePercentages_WithNullValues() {
    	Mockery mockingContext = new Mockery();
        final KeyedValues values = mockingContext.mock(KeyedValues.class);
        mockingContext.checking(new Expectations() {
            {
            	allowing(values).getItemCount();
                will(returnValue(4));
                allowing(values).getKey(0);
                will(returnValue(0));
                allowing(values).getValue(0);
                will(returnValue(null));
                allowing(values).getKey(1);
                will(returnValue(1));
                allowing(values).getValue(1);
                will(returnValue(4.5));
                allowing(values).getKey(2);
                will(returnValue(2));
                allowing(values).getValue(2);
                will(returnValue(null));
                allowing(values).getKey(3);
                will(returnValue(3));
                allowing(values).getValue(3);
                will(returnValue(3.5));
            }
        });
        KeyedValues result = DataUtilities.getCumulativePercentages(values);
        
        Number[] expectedResults = {0.0, (4.5/8.0), (4.5/8.0), 1.0};
        Number [] actualResults = new Number[4];
        for(int i = 0; i < result.getItemCount(); i++) {
        	actualResults[i] = result.getValue(i);
        }
        
        assertArrayEquals("The KeyedValues object contains cumulative percentages 0.5625 and 0.5625",expectedResults, actualResults);
    }
    
    
    @Test(expected = IllegalArgumentException.class)
    public void getCumulativePercentages_NullData() {
    	KeyedValues values = null;
    	DataUtilities.getCumulativePercentages(values);
    }
    
    
    //-----------------------------------------------------------------------------------------------------------------------------//

    private DataUtilities exampleDataUtilities;
    @BeforeClass public static void setUpBeforeClass() throws Exception {
    }
   

    //Normal Input with non-null values
    @Test
    public void createNumberArray_NonNullDataInput() {
        //Sample Input with Non Null Data Input
        double [] data = { 13.4, 12.00004, 0.001, 1235.0, 234};
        Number [] result = DataUtilities.createNumberArray(data);
        Number [] expected = { 13.4, 12.00004, 0.001, 1235.0, 234.0};
        
        assertArrayEquals("The data input should include non-null double values", expected, result);
    }

    //Too Large value. Checked for Conversion. 
    @Test
    public void createNumberArray_ExtremeDataInput() {
        //Sample Input with Extreme (large/small) doubles
        double [] data = { Double.MAX_VALUE, Double.MAX_VALUE};
        Number [] result = DataUtilities.createNumberArray(data);
        Number [] expected = { Double.MAX_VALUE, Double.MAX_VALUE};
        
        assertArrayEquals("Extreme values should be allowed (large doubles)", expected, result);
    }
    
    //Too small value. Checked for Conversion. 
    @Test
    public void createNumberArray_SmallNegativeDataInput() {
        //Sample Input with Extreme (large/small) doubles
        double [] data = { Double.MIN_VALUE, Double.MIN_VALUE};
        Number [] result = DataUtilities.createNumberArray(data);
        Number [] expected = { Double.MIN_VALUE, Double.MIN_VALUE};
        
        assertArrayEquals("Extreme values should be allowed (small doubles)", expected, result);
    }
    
    //Invalid Array is used as an Argument. 
    @Test
     public void createNumberArray_EmptyArray() {
         //Sample Input with Null. Expect a null return
         double [] data = {};
         Number [] result = DataUtilities.createNumberArray(data);
         assertNotNull("Checking that createNumberArray can throw exception when a null array is sent",  result);
     }
    
    //Array is set to null
	@Test(expected = IllegalArgumentException.class)
	public void createNumberArray_NullValue() {
		double[] data = null;
		
		DataUtilities.createNumberArray(data);
	}
	
    @Test
    public void createNumberArray_NaNValue() {
        //Sample Input with NaN Data Input
        double [] data = {Double.NaN, Double.NaN, Double.NaN};
        Number [] result = DataUtilities.createNumberArray(data);
        Number [] expected = {Double.NaN, Double.NaN, Double.NaN};
        
        assertArrayEquals("The data input should include NaN double values", expected, result);
    }
    
    @Test
    public void createNumberArray_PositiveInf() {
        //Sample Input with Positive Infinity Data Input
        double [] data = { Double.POSITIVE_INFINITY, Double.POSITIVE_INFINITY};
        Number [] result = DataUtilities.createNumberArray(data);
        Number [] expected = { Double.POSITIVE_INFINITY, Double.POSITIVE_INFINITY};
        
        assertArrayEquals("The data input should include Positive Infinity double values", expected, result);
    }
    
    @Test
    public void createNumberArray_NegativeInf() {
        //Sample Input with Negative Infinity Data Input
        double [] data = { Double.NEGATIVE_INFINITY, Double.NEGATIVE_INFINITY, Double.NEGATIVE_INFINITY};
        Number [] result = DataUtilities.createNumberArray(data);
        Number [] expected = { Double.NEGATIVE_INFINITY, Double.NEGATIVE_INFINITY, Double.NEGATIVE_INFINITY};
        
        assertArrayEquals("The data input should include Negative Infinity double values", expected, result);
    }

    
    //-----------------------------------------------------------------------------------------------------------------------------//
	
    @Test
    public void calculateColumnTotal_NullDataValidColumn() {
        Mockery mockingContext = new Mockery();
        final Values2D values = mockingContext.mock(Values2D.class);
        mockingContext.checking(new Expectations() {
            {
                one(values).getRowCount();
                will(returnValue(3));
                one(values).getValue(0, 0);
                will(returnValue(null));
                one(values).getValue(1, 0);
                will(returnValue(null));
                one(values).getValue(2, 0);
                will(returnValue(null));
            }
        });
        double result = DataUtilities.calculateColumnTotal(values, 0);
        assertEquals("The calculated total is 0.0",result, 0.0, .000000001d);
    }
    
    @Test
    public void calculateColumnTotal_ValidDataValidColumn() {
        Mockery mockingContext = new Mockery();
        final Values2D values = mockingContext.mock(Values2D.class);
        mockingContext.checking(new Expectations() {
            {
                one(values).getRowCount();
                will(returnValue(3));
                one(values).getValue(0, 0);
                will(returnValue(5.67));
                one(values).getValue(1, 0);
                will(returnValue(5.2));
                one(values).getValue(2, 0);
                will(returnValue(1.2));
                one(values).getValue(0, 1);
                will(returnValue(1.7));
            }
        });
        double result = DataUtilities.calculateColumnTotal(values, 0);
        assertEquals("The calculated total is 12.07",result, 12.07, .000000001d);
    }
    
    @Test
    public void calculateColumnTotal_ValidDataExtremeValuesValidColumn() {
        Mockery mockingContext = new Mockery();
        final Values2D values = mockingContext.mock(Values2D.class);
        mockingContext.checking(new Expectations() {
            {
                one(values).getRowCount();
                will(returnValue(2));
                one(values).getValue(0, 2);
                will(returnValue(Double.MAX_VALUE));
                one(values).getValue(1, 2);
                will(returnValue(Double.MAX_VALUE));
                one(values).getValue(2, 2);
            }
        });
        double result = DataUtilities.calculateColumnTotal(values, 2);
        assertEquals("The calculated total is Double.MAX_VALUE+Double.MAX_VALUE",result, (Double.MAX_VALUE+Double.MAX_VALUE), .000000001d);
    }

    @Test
    public void calculateColumnTotal_ValidDataNegativeValueValidColumn() {
        Mockery mockingContext = new Mockery();
        final Values2D values = mockingContext.mock(Values2D.class);
        mockingContext.checking(new Expectations() {
            {
                one(values).getRowCount();
                will(returnValue(3));
                one(values).getValue(0, 1);
                will(returnValue(Double.MIN_VALUE));
                one(values).getValue(1, 1);
                will(returnValue(Double.MIN_VALUE));
                one(values).getValue(2, 1);
                will(returnValue(Double.MIN_VALUE));
            }
        });
        double result = DataUtilities.calculateColumnTotal(values, 1);
        assertEquals("The calculated total is Double.MIN_VALUE+Double.MIN_VALUE+Double.MIN_VALUE"
        		,result, (Double.MIN_VALUE+Double.MIN_VALUE+Double.MIN_VALUE), .000000001d);
    }
    
    @Test(expected = IndexOutOfBoundsException.class)
    public void calculateColumnTotal_ValidDataInvalidColumnAboveUpperBoundary() {
    	DefaultKeyedValues2D test = new DefaultKeyedValues2D();
    	test.addValue(1, 0, 0);
        Values2D values = test;
        
        DataUtilities.calculateColumnTotal(values, 5);
    }
    

    @Test(expected = IndexOutOfBoundsException.class)
    public void calculateColumnTotal_ValidDataInvalidColumnBelowLowerBoundary() {
    	DefaultKeyedValues2D test = new DefaultKeyedValues2D();
    	test.addValue(1, 0, 0);
        Values2D values = test;
        
        DataUtilities.calculateColumnTotal(values, -2);
    }

    @Test(expected = IllegalArgumentException.class)
    public void calculateColumnTotal_NullDataArgument(){
	double result = DataUtilities.calculateColumnTotal(null, 0);
    }
	
	 //-----------------------------------------------------------------------------------------------------------------------------//
    
    Mockery context;

    //Checking whether calculateColumnTotal works as expected with non-null values
    @Before
    public void setUp_NonNullAndValidColumn() throws Exception { 
        context = new Mockery();
    }
    
    @Test
    public void calculateColumnTotal_NonNullAndValidColumn(){
        final org.jfree.data.Values2D values = context.mock(org.jfree.data.Values2D.class);

        int dataRowCount = 3;
        int column = 2;

        context.checking(new Expectations() {{
            one(values).getRowCount();
            will(returnValue(dataRowCount));
            one(values).getValue(0,column);
            will(returnValue(2.4));
            one(values).getValue(1,column);
            will(returnValue(7.2));
            one(values).getValue(2,column);
            will(returnValue(9.4));
        }});
        
        int [] validRows = {0,1,2};

        double result = DataUtilities.calculateColumnTotal(values, column, validRows);
        assertEquals("Method cannot support a valid Values2D object, column and valid rows", result, 19.0, .000000001d);
    }

    @After
    public void tearDown_NonNullAndValidColumn() throws Exception {
        context = null;
    }

    //Test a column value not allowed by Value 2D Array. ie < 0
    @Before
     public void setUp_TooSmallColumn() throws Exception { 
         context = new Mockery();
     }
     
     @Test(expected = IndexOutOfBoundsException.class)
     public void calculateColumnTotal_TooSmallColumn(){
         DefaultKeyedValues2D test = new DefaultKeyedValues2D();
         test.addValue(1, 0, 0);
         org.jfree.data.Values2D values = (org.jfree.data.Values2D) test;
 
         int [] validRows = {0};
         DataUtilities.calculateColumnTotal((org.jfree.data.Values2D) values, -1, validRows);
     }

     @After
     public void tearDown_TooSmallColumn() throws Exception {
         context = null;
     }
 
    //Testing column that is too large. Exception should be thrown. 
    @Test(expected = IndexOutOfBoundsException.class)
    public void calculateColumnTotal_TooLargeColumn(){
        DefaultKeyedValues2D test = new DefaultKeyedValues2D();
        test.addValue(1, 0, 0);
        org.jfree.data.Values2D values = (org.jfree.data.Values2D) test;

        int [] validRows = {0};
        DataUtilities.calculateColumnTotal((org.jfree.data.Values2D) values, 5, validRows);
    }

    //Upper Boundary Valid Rows check. Viewing
    @Before
    public void setUp_ValidRowsExceeded() throws Exception { 
        context = new Mockery();
    }
    
    // @Test(expected = IndexOutOfBoundsException.class)
    // public void calculateColumnTotal_ValidRowsExceeded(){
    //     final org.jfree.data.Values2D values = context.mock(org.jfree.data.Values2D.class);

    //     int dataRowCount = 3;
    //     int column = 2;

    //     context.checking(new Expectations() {{
    //         one(values).getRowCount();
    //         will(returnValue(dataRowCount));
    //     }});
        
    //     int [] validRows = {4};
    //     DataUtilities.calculateColumnTotal(values, 3, validRows);
    // }

    @After
    public void tearDown_ValidRowsExceeded() throws Exception {
        context = null;
    }

    //Testing whether Invalid Row (<0) is accepted. Exception thrown by methods means test passes
    @Test
    public void calculateColumnTotal_RowsBelowZero(){
        DefaultKeyedValues2D test = new DefaultKeyedValues2D();
        test.addValue(1, 0, 0);
        org.jfree.data.Values2D values = (org.jfree.data.Values2D) test;

        int [] validRows = {-1};

        try{
           double value = DataUtilities.calculateColumnTotal((org.jfree.data.Values2D) values, 0, validRows);
           assertNull("Calculation should throw exception to out of bounds value (<0) for Rows",value);
        } catch (Exception e){

        }         
    }
 
	 //-----------------------------------------------------------------------------------------------------------------------------//
    
    @Test
    public void Equal_ValidPositiveValuesArrays() {
    	double[][] array1 = {{15.0, 12.67}, {5.6, 7.8}};
    	double[][] array2 = {{15.0, 12.67}, {5.6, 7.8}};
    	
    	assertTrue("Equal method is expected to return true with positive values.", DataUtilities.equal(array1, array2));
    }
    
    @Test
    public void Equal_ValidNegativeValuesArrays() {
    	double[][] array1 = {{-15.0, -12.67}, {-5.6, -7.8}};
    	double[][] array2 = {{-15.0, -12.67}, {-5.6, -7.8}};
    	
    	assertTrue("Equal method is expected to return true with negative values.", DataUtilities.equal(array1, array2));
    }
    
    @Test
    public void Equal_NullArrays() {
    	double[][] array1 = null;
    	double[][] array2 = null;
    	
    	assertTrue("Equal method is expected to return true with null values.", DataUtilities.equal(array1, array2));
    }

    
    @Test
    public void Equal_ExtemeValueArrays() {
    	double[][] array1 = {{Double.MAX_VALUE, 12.67}, {Double.MAX_VALUE, Double.MAX_VALUE}};
    	double[][] array2 = {{Double.MAX_VALUE, 12.67}, {Double.MAX_VALUE, Double.MAX_VALUE}};
    	
    	assertTrue("Equal method is expected to return true with extreme values.", DataUtilities.equal(array1, array2));
    }
    
    @Test
    public void Equal_SmallNegativeValueArrays() {
    	double[][] array1 = {{Double.MIN_VALUE, Double.MIN_VALUE}, {Double.MIN_VALUE, Double.MIN_VALUE}};
    	double[][] array2 = {{Double.MIN_VALUE, Double.MIN_VALUE}, {Double.MIN_VALUE, Double.MIN_VALUE}};
    	
    	assertTrue("Equal method is expected to return true with very small negative values.", DataUtilities.equal(array1, array2));
    }
    
    @Test
    public void Equal_EmptyArrays() {
    	double[][] array1 = {{}, {}};
    	double[][] array2 = {{}, {}};
    	
    	assertTrue("Equal method is expected to return true with empty arrays.", DataUtilities.equal(array1, array2));
    }

    
    @Test
    public void Equal_NaNValueArrays() {
    	double[][] array1 = {{Double.NaN, Double.NaN}, {Double.NaN, Double.NaN}};
    	double[][] array2 = {{Double.NaN, Double.NaN}, {Double.NaN, Double.NaN}};
    	
    	assertTrue("Equal method is expected to return true with NaN values.", DataUtilities.equal(array1, array2));
    }
    
    @Test
    public void Equal_PositiveInfValueArrays() {
    	double[][] array1 = {{Double.POSITIVE_INFINITY, Double.POSITIVE_INFINITY}};
    	double[][] array2 = {{Double.POSITIVE_INFINITY, Double.POSITIVE_INFINITY}};
    	
    	assertTrue("Equal method is expected to return true with positive infinity values.", DataUtilities.equal(array1, array2));
    }
    
    @Test
    public void Equal_NegativeInfValueArrays() {
    	double[][] array1 = {{Double.NEGATIVE_INFINITY, Double.NEGATIVE_INFINITY}};
    	double[][] array2 = {{Double.NEGATIVE_INFINITY, Double.NEGATIVE_INFINITY}};
    	
    	assertTrue("Equal method is expected to return true with negative infinity values.", DataUtilities.equal(array1, array2));
    }
    
	 //-----------------------------------------------------------------------------------------------------------------------------//
    
    @Test(timeout=100)
    public void Clone_ValidPositiveValuesArrayData() {
    	double [][] expected = {{15.0, 12.67}, {5.6, 7.8}};
    	double [][] result = DataUtilities.clone(expected);
    	
    	assertArrayEquals("Clone method returns double 2D array containing {15.0, 12.67}, {5.6, 7.8}.", result, expected);
    }
    
    @Test(timeout=100)
    public void Clone_ValidNegativeValuesArrayData() {
    	double [][] expected = {{-15.0, -12.67}, {-5.6, -7.8}};
    	double [][] result = DataUtilities.clone(expected);
    	
    	assertArrayEquals("Clone method returns double 2D array containing {-15.0, -12.67}, {-5.6, -7.8}.", result, expected);
    }

    @Test(timeout=100)
    public void Clone_PositiveArrayWithNullRow(){
	double[][] expected = {{14.3,0.002},null};
	double[][] result = DataUtilities.clone(expected);

	assertArrayEquals("Clone method expects a double 2D array containing {14.3, 0.002}, null", result, expected);
    }
    
    @Test(expected = IllegalArgumentException.class)
    public void Clone_NullArrayData() {
    	double [][] expected = null;
    	DataUtilities.clone(expected);
    }
    
    @Test(timeout=100)
    public void Clone_EmptyArrayData() {
    	double [][] expected = {{}};
    	double [][] result = DataUtilities.clone(expected);
    	
    	assertArrayEquals("Clone method returns double 2D array that is empty.", result, expected);
    }
    
    @Test(timeout=100)
    public void Clone_ExtremeValuesArrayData() {
    	double [][] expected = {{Double.MAX_VALUE, 12.67}, {Double.MAX_VALUE, Double.MAX_VALUE}};
    	double [][] result = DataUtilities.clone(expected);
    	
    	assertArrayEquals("Clone method returns double 2D array containing {Double.MAX_VALUE, 12.67}, {Double.MAX_VALUE, Double.MAX_VALUE}.", result, expected);
    }
    
    @Test(timeout=100)
    public void Clone_SmallNegativeValuesArrayData() {
    	double [][] expected = {{Double.MIN_VALUE, Double.MIN_VALUE}, {Double.MIN_VALUE, Double.MIN_VALUE}};
    	double [][] result = DataUtilities.clone(expected);
    	
    	assertArrayEquals("Clone method returns double 2D array containing {Double.MIN_VALUE, Double.MIN_VALUE}, {Double.MIN_VALUE, Double.MIN_VALUE}.", result, expected);
    }
    
    @Test(timeout=100)
    public void Clone_NaNArrayData() {
    	double [][] expected = {{Double.NaN, Double.NaN}};
    	double [][] result = DataUtilities.clone(expected);
    	
    	assertArrayEquals("Clone method returns double 2D array containing {Double.POSITIVE_INFINITY, Double.POSITIVE_INFINITY}.", result, expected);
    }

    @Test(timeout=100)
    public void Clone_PositiveInfArrayData() {
    	double [][] expected = {{Double.POSITIVE_INFINITY, Double.POSITIVE_INFINITY}};
    	double [][] result = DataUtilities.clone(expected);
    	
    	assertArrayEquals("Clone method returns double 2D array containing {Double.POSITIVE_INFINITY, Double.POSITIVE_INFINITY}.", result, expected);
    }
    
    @Test(timeout=100)
    public void Clone_NegativeInfArrayData() {
    	double [][] expected = {{Double.NEGATIVE_INFINITY, Double.NEGATIVE_INFINITY}};
    	double [][] result = DataUtilities.clone(expected);
    	
    	assertArrayEquals("Clone method returns double 2D array containing {Double.NEGATIVE_INFINITY, Double.NEGATIVE_INFINITY}.", result, expected);
    }
    
    //-----------------------------------------------------------------------------------------------------------------------------//

    @Test 
    public void Equal_UnequalNullArrayPositiveValuesArray(){
	double[][] array1 = {{13.0, 15},{22,3}};
	double[][] array2 = null;

	assertFalse("Equal method is expected to return false with a null value and a positive value.", DataUtilities.equal(array1, array2));
    }

    @Test 
    public void Equal_UnequalNullArrayNegativeValueNullArray(){
	double[][] array1 = null;
	double[][] array2 = {{-1900.4, -0.2},{-9.23,-10.001}};

	assertFalse("Equal method is expected to return false with a null value and a negative value.", DataUtilities.equal(array1, array2));
    }

    @Test
    public void Equal_UnequalSameIndex0DifferentIndex1(){
	double[][] array1 = {{2},{4.0}};
	double[][] array2 = {{3},{4.0}};

	assertFalse("Equal method is expected to return false with arrays with different values at index 0 and the same value at index 1", DataUtilities.equal(array1, array2));
    }

    @Test
    public void Equal_UnequalArraysLength6CompletelyDifferentValues(){
	double[][] array1 = {{1.0},{2.0},{3.0},{4.0},{5.0},{6.0}};
	double[][] array2 = {{-1.0},{-2.0},{-3.0},{-4.0},{-5.0},{-6.0}};

	assertFalse("Equal method is expected to return false with arrays of size [6][1] and no similar values", DataUtilities.equal(array1, array2));
    }

    @Test
    public void Equal_UnequalLengthPositiveArrays(){
	double[][] array1 = {{1.0},{58.0},{12.4}};
	double[][] array2 = {{1.0},{58.0}};

	assertFalse("Equal method is expected to return false with two positive arrays of different lengths", DataUtilities.equal(array1, array2));
    }
    
    @Test
    public void Equal_UnequalLengthNegativeValueEmptyArray(){
	double[][] array1 = {{},{}};
	double[][] array2 = {{-1,-2},{-4,-5}};

	assertFalse("Equal method is expected to return false with a negative array and an empty array", DataUtilities.equal(array1, array2));
    }	

    @Test
    public void Equal_UnequalPositiveValueArrays() {
    	double[][] array1 = {{12.55, 1.0}, {13.4, 7.8}};
    	double[][] array2 = {{12.67, 1.1}, {16.4, 9.8}};
    	
    	assertFalse("Equal method is expected to return false with arrays containing different values.", DataUtilities.equal(array1, array2));
    }

    @Test
    public void Equal_UnequalLengthPositiveArraysALengthLessThanBLength(){
	double[][] array1 = {{12.1},{2.4}};
	double[][] array2 = {{12.1},{2.4},{14.3},{0.22}};

    	assertFalse("Equal method is expected to return false with arrays of different lengths and array2 being shorter than array1.", DataUtilities.equal(array1, array2));
    }
    
    //added mutation tests
    //150:
    @Test(expected = IllegalArgumentException.class)
    public void calculateColumnTotalOverloaded_NullDataArgument(){
    	int [] rows = {0};
    	double result = DataUtilities.calculateColumnTotal(null, 0, rows);
    }
    //201:
    @Test(expected = IllegalArgumentException.class)
    public void calculateRowTotalOverloaded_NullDataArgument(){
    	int [] columns = {0};
    	double result = DataUtilities.calculateRowTotal(null, 0, columns);
    }
    //85, 13:
	@Test
	public void Equal_MiddleArrayNotEqual() {
		double [][] array1 = {{7,5},{4,2},{6,9}};
		double [][] array2 = {{7,5},{2,4},{6,9}};

		assertFalse("Equal method is expected to return false.", DataUtilities.equal(array1, array2));
	}
	
	@Test
	public void Equal_OuterArrayNotEqual() {
		double [][] array1 = {{7,5},{4,2},{6,9}};
		double [][] array2 = {{5,7},{4,2},{9,6}};

		assertFalse("Equal method is expected to return false.", DataUtilities.equal(array1, array2));
	}
    
	
    @Test
    public void calculateColumnTotal_ValidDataValidRowValidColumnsUnusedRow() {
        Mockery mockingContext = new Mockery();
        final Values2D values = mockingContext.mock(Values2D.class);
        mockingContext.checking(new Expectations() {
            {
                one(values).getColumnCount();
                will(returnValue(3));
                one(values).getRowCount();
                will(returnValue(3));
                one(values).getValue(0, 0);
                will(returnValue(4));
                one(values).getValue(1, 1);
                will(returnValue(5));
                one(values).getValue(2, 1);
                will(returnValue(9));
            }
        });
        int[] rows = {1, 2, 5};
        double result = DataUtilities.calculateColumnTotal(values, 1, rows);
        assertEquals("The calculated total is 14.0",result, 14.0, .000000001d);
    }
	
    
    @Test
    public void calculateColumnTotal_NullDataAndValidDataValidColumnValidRows() {
        Mockery mockingContext = new Mockery();
        final Values2D values = mockingContext.mock(Values2D.class);
        mockingContext.checking(new Expectations() {
            {
                one(values).getColumnCount();
                will(returnValue(3));
                one(values).getRowCount();
                will(returnValue(3));
                one(values).getValue(0, 0);
                will(returnValue(null));
                one(values).getValue(1, 0);
                will(returnValue(2.4));
                one(values).getValue(2, 0);
                will(returnValue(null));
            }
        });
        int[] rows = {0, 1, 2};
        double result = DataUtilities.calculateColumnTotal(values, 0, rows);
        assertEquals("The calculated total is 2.4",result, 2.4, .000000001d);
    }
    
    @Test
    public void calculateRowTotal_NullDataAndValidDataValidRowValidColumns() {
        Mockery mockingContext = new Mockery();
        final Values2D values = mockingContext.mock(Values2D.class);
        mockingContext.checking(new Expectations() {
            {
                one(values).getColumnCount();
                will(returnValue(3));
                one(values).getRowCount();
                will(returnValue(3));
                one(values).getValue(0, 0);
                will(returnValue(null));
                one(values).getValue(0, 1);
                will(returnValue(2.4));
                one(values).getValue(0, 2);
                will(returnValue(null));
            }
        });
        int[] columns = {0, 1, 2};
        double result = DataUtilities.calculateRowTotal(values, 0, columns);
        assertEquals("The calculated total is 2.4",result, 2.4, .000000001d);
    }

    @Test
    public void calculateRowTotal_ValidDataValidRowValidColumnsUnusedColumn() {
        Mockery mockingContext = new Mockery();
        final Values2D values = mockingContext.mock(Values2D.class);
        mockingContext.checking(new Expectations() {
            {
                one(values).getColumnCount();
                will(returnValue(3));
                one(values).getRowCount();
                will(returnValue(3));
                one(values).getValue(0, 0);
                will(returnValue(4));
                one(values).getValue(1, 1);
                will(returnValue(5));
                one(values).getValue(1, 2);
                will(returnValue(9));
            }
        });
        int[] columns = {1, 2, 5};
        double result = DataUtilities.calculateRowTotal(values, 1, columns);
        assertEquals("The calculated total is 14.0",result, 14.0, .000000001d);
    }
    
    @Test(expected = NullPointerException.class)
    public void calculateRowTotal_ValidDataValidRowNullColumns(){
    	Mockery mockingContext = new Mockery();
        final Values2D values = mockingContext.mock(Values2D.class);
        mockingContext.checking(new Expectations() {
            {
                one(values).getColumnCount();
                will(returnValue(3));
                one(values).getRowCount();
                will(returnValue(3));
                one(values).getValue(0, 0);
                will(returnValue(4));
                one(values).getValue(1, 1);
                will(returnValue(5));
                one(values).getValue(1, 2);
                will(returnValue(9));
            }
        });
        double result = DataUtilities.calculateRowTotal(values, 1, null);
    }
    
    
    @Test
    public void calculateColumnTotal_ValidDataValidRowValidColumnsRowUnusedColumnCountNotSet() {
        Mockery mockingContext = new Mockery();
        final Values2D values = mockingContext.mock(Values2D.class);
        mockingContext.checking (new Expectations () {
            {
                one(values).getRowCount ();
                will(returnValue(3));
                
                one(values).getValue(0,0);
                will(returnValue(4.4));
                one(values).getValue(0,1);
                will(returnValue(3.1));
                one(values).getValue(0,2);
                will(returnValue(8.0));
                
                one(values).getValue(1,0);
                will(returnValue(21.0));
                one(values).getValue(1,1);
                will(returnValue(1.1));
                one(values).getValue(1,2);
                will(returnValue(9.5));
                
                one(values).getValue(2,0);
                will(returnValue(5.3));
                one(values).getValue(2,1);
                will(returnValue(1.9));
                one(values).getValue(2,2);
                will(returnValue(1.94));
            }
        });

        int [] rows = {0,3};
        double result = DataUtilities.calculateColumnTotal (values, 1, rows);
        assertEquals ("The calculated total is 3.1", result, 3.1, 0.000000001d);
    }

    @Test
    public void calculateRowTotal_ValidDataValidColumnValidRowsColumnUnusedRowCountNotSet() {
        Mockery mockingContext = new Mockery();
        final Values2D values = mockingContext.mock(Values2D.class);
        mockingContext.checking (new Expectations () {
            {
                one(values).getColumnCount ();
                will(returnValue(3));
                
                one(values).getValue(0,0);
                will(returnValue(4.4));
                one(values).getValue(1,0);
                will(returnValue(3.1));
                one(values).getValue(2,0);
                will(returnValue(8.0));
                
                one(values).getValue(0,1);
                will(returnValue(21.0));
                one(values).getValue(1,1);
                will(returnValue(1.1));
                one(values).getValue(2,1);
                will(returnValue(9.5));
                
                one(values).getValue(0,2);
                will(returnValue(5.3));
                one(values).getValue(1,2);
                will(returnValue(1.9));
                one(values).getValue(2,2);
                will(returnValue(1.94));
            }
        });

        int [] columns = {0,3};
        double result = DataUtilities.calculateRowTotal (values, 1, columns);
        assertEquals ("The calculated total is 3.1", result, 3.1, 0.000000001d);
    }

    @Test
    public void calculateColumnTotalCheckArrayChanges () {
        Mockery mockingContext = new Mockery ();
        final Values2D values = mockingContext.mock(Values2D.class);
        mockingContext.checking (new Expectations () {
            {
            	one(values).getColumnCount();
                will(returnValue(3));
                one(values).getRowCount();
                will(returnValue(3));
                one(values).getValue(0, 0);
                will(returnValue(null));
                one(values).getValue(1, 0);
                will(returnValue(2.4));
                one(values).getValue(2, 0);
                will(returnValue(null));
            }
        });

        int [] result = {0,1,2};
        int [] rows = {0,1,2};
        DataUtilities.calculateColumnTotal (values, 0, rows);
        
        assertArrayEquals (result, rows);
    }
    
    @Test
    public void calculateRowTotalCheckArrayChanges () {
        Mockery mockingContext = new Mockery ();
        final Values2D values = mockingContext.mock(Values2D.class);
        mockingContext.checking (new Expectations () {
            {
                one(values).getColumnCount();
                will(returnValue(3));
                one(values).getRowCount();
                will(returnValue(3));
                one(values).getValue(0, 0);
                will(returnValue(null));
                one(values).getValue(0, 1);
                will(returnValue(2.4));
                one(values).getValue(0, 2);
                will(returnValue(null));
            }
        });

        int [] result = {0,1,2};
        int [] rows = {0,1,2};
        DataUtilities.calculateRowTotal (values, 0, rows);
        
        assertArrayEquals (result, rows);
    }
    
    @Test
    public void calculateRowTotalValidDataTestTwice(){
        Mockery mockingContext = new Mockery();
        final Values2D values = mockingContext.mock(Values2D.class);
        mockingContext.checking(new Expectations() {
            {
            	atLeast(1).of(values).getColumnCount();
                will(returnValue(3));
                atLeast(1).of(values).getValue(0, 0);
                will(returnValue(5.67));
                atLeast(1).of(values).getValue(0, 1);
                will(returnValue(5.2));
                atLeast(1).of(values).getValue(0, 2);
                will(returnValue(1.2));
            }
        });
        DataUtilities.calculateRowTotal(values, 0);
        assertEquals("The calculated total is 12.07",DataUtilities.calculateRowTotal(values, 0), 12.07, .000000001d);
    }
    
    @Test(timeout=100)
    public void calculateColumnTotalValidDataTestTwice(){
        Mockery mockingContext = new Mockery();
        final Values2D values = mockingContext.mock(Values2D.class);
        mockingContext.checking(new Expectations() {
            {
            	atLeast(1).of(values).getRowCount();
                will(returnValue(3));
                atLeast(1).of(values).getValue(0, 0);
                will(returnValue(5.67));
                atLeast(1).of(values).getValue(1, 0);
                will(returnValue(5.2));
                atLeast(1).of(values).getValue(2, 0);
                will(returnValue(1.2));
            }
        });
        DataUtilities.calculateColumnTotal(values, 0);
        assertEquals("The calculated total is 12.07",DataUtilities.calculateColumnTotal(values, 0), 12.07, .000000001d);
    }
    
    @Test
    public void calculateRowTotal_ValidDataValidRowValidColumnsTestedTwice() {
        Mockery mockingContext = new Mockery();
        final Values2D values = mockingContext.mock(Values2D.class);
        mockingContext.checking(new Expectations() {
            {
            	atLeast(1).of(values).getColumnCount();
                will(returnValue(3));
                atLeast(1).of(values).getRowCount();
                will(returnValue(3));
                atLeast(1).of(values).getValue(0, 0);
                will(returnValue(4));
                atLeast(1).of(values).getValue(1, 1);
                will(returnValue(5));
                atLeast(1).of(values).getValue(1, 2);
                will(returnValue(9));
            }
        });
        int[] columns = {1, 2};
        DataUtilities.calculateRowTotal(values, 1, columns);
        assertEquals("The calculated total is 14.0",DataUtilities.calculateRowTotal(values, 1, columns), 14.0, .000000001d);
    }

	
    //-----------------------------------------------------------------------------------------------------------------------------//

    @After
    public void tearDown() throws Exception {
    }
}
```

# A discussion on the effect of equivalent mutants on mutation score accuracy

Since equivalent mutants cannot be caught, it is challenging to increase the mutation score on classes that only have equivalent mutants remaining. Thus, equivalent mutants reduce the accuracy of the mutation score. For example, `DataUtilities` class has a score of 94% in mutation score. It is not possible to increase this further as the uncaught mutants are equivalent mutants. Thus, the score is not a reflection of the testability of the software. Although, changing the process of generating mutants can prevent equivalent mutants from occurring, thus, increasing the score. For our lab, this was not the case, and as a result, 94% was the highest score we could get. 

# A discussion of what could have been done to improve the mutation score of the test suites

In order to improve the mutation score of test suites, we can adjust the source codes to catch errors when mutants are injected. In the `Range` and `DataUtilities` class, this was the method used to increase the score. However, this does become a challenge for equivalent mutants. In `DataUtilities` specifically, the remaining mutants are equivalent mutants. 

# Why do we need mutation testing? Advantages and disadvantages of mutation testing

Mutation testing is an important tool to find weak points in the program that cannot be found in traditional black-box testing and white box testing (statement coverage). This is evident in the initial percentages of mutation coverage where the results of each class showed weaknesses in the `DataUtilities` and `Range` class. Mutation testing can discover faults in places that aren’t covered by statement coverage since they may not be computed. Issues such as these can lead to future development problems and affect code in the long run. 

Alternatively, Mutation testing can be a time-consuming process that can feel unnecessary. From assignment 3, we know that the program is well tested and we cover the use-cases of the program.  As a result, we don’t see the direct effects of mutation testing from initial inspection. Another reason why mutation testing is time-consuming is that the mutant program must be tested separately from another, and as a result, can be tiring for us to perform. It is also challenging to increase the mutation score, thus, making mutation testing an expensive process. 

# Explain your SELENUIM test case design process

We started off by brainstorming different use cases of the application that we intended to test. Then for each use case, we determined different user stories that consist of different inputs and resultant behaviours of the application.

| Functionality | Test # 1 | Test # 2 |
| --- | --- | --- |
| Add Dell Item to Cart | Looking for success | Test In Store Pickup |
| French Site | Looking for success |  |
| Search for iPhone | Looking for success | Look using 1Ph0ne |
| Store Locator | Looking for success  | Looking for no results |
| Login | Looking for success (can fail due to captcha) | Looking for fail |
| Chat | Looking for success via an announcement popup | Look for failure due to email |
| Signing up for announcement | Looking for success with the message |  |
| Price Range | Verifying at least 1 item exists in the price range of $250 - $255 |  |

# Explain the use of assertions and checkpoints

Assertions and checkpoints were utilized towards the end of each test to verify whether specific elements or text appear. For example, in the Chat test, we check for a specific notification that temporarily appears if a chat has started with Bestbuy’s assistant. DOM Elements, failed action messages (ie login failure), and the availability of items on the website were some of the checkpoints utilized in each test. These verification points help check whether tests past and afterwords, tests would reset to ensure future tests can be completed as expected. 

| Functionality | Verification Point Type |
| --- | --- |
| Add Dell Item to Cart | assertText |
| French Site | assertText |
| Search for iPhone | assertText |
| Store Locator | assertText, assertElementPresent |
| Login | assertText |
| Chat | assertElementPresent |
| Signing up for announcement | assertText |
| Price Range | assertText |

# How did you test each functionality with different test data

Specific functionalities were tested with different types of data. For example, for the chat functionality, we utilized different passwords with the same username. This username was created by us to test Bestbuy’s account functionalities. By testing different passwords, we ensured the system can handle various cases a user may attempt. Our test cases reflect these. Similarly, for the Store Locator functionality, we look for stores that exist and do not exist to ensure the software can handle these inputs. The Selenium IDE can help us check for errors that are supposed to happen, for example, an incorrect password. With this approach, we can test functionality with various test data. 

# Discuss advantages and disadvantages of Selenium vs. Sikulix

Utilizing Selenium allows the testing tool to interact with specific components within a page and check that components are displayed with the correct behaviour. This can be extremely helpful when testing applications that are responsive on different devices. In contrast, Sikulix interacts with the application and assesses the status of the page utilizing image recognition. This tool may be able to test visual elements but is unable to test more of the logic of the page itself and how the components within the DOM are rendered.

# How the teamwork/effort was divided and managed

The group initially worked together to setup the environment for Pitest in the mutation testing phase of this assignment. We compared, contrasted and tried to understand the mutation score based on the Pitest documentation. Afterwards, we brainstormed a test design plan to increase our mutation score and worked in pairs to increase each class's scores. After this was complete, we moved on to the Selenium. Unlike mutation testing that required a complex environment setup, the Selenium chrome extension is accessible. The 4 of us tested various features on [Bestbuy.ca](http://Bestbuy.ca) and documented each functionality and test to prevent overlap. At the end of this, we went back to verify each section by pair. Robert and Risat focused more on the Selenium while Brooke and Amnah focused more on Pitest. The report was divided evenly and proof-read by each other. 

# Difficulties encountered, challenges overcome, and lessons learned

One of the challenges we encountered with Selenium web testing was the use of CAPTCHAs and other bot mitigation measures implemented by websites. Since these tools are meant to prevent robots from gaining access to the site which meant we were unable to develop test cases that are able to handle these. We will see that sometimes these tests fail, and as a result, can be challenging to test in all cases. Another difficulty encountered was testing features on websites that are time-dependant on humans. The chat feature requires another individual to exist on the other side, and since we cannot guarantee that Bestbuy has their assistants active 24/7, testing this feature can be difficult. 

# Comments/feedback on the lab itself

Pitest was slow to run, making it tedious and resource-intensive to do the lab. Because of the long wait before getting mutation testing results back, even when testing was limited to a single class, it was challenging to evaluate which actions were best suited for killing particular mutants. This was made especially complex because it was not possible to look at the results of a previous Pitest execution without the manual copying of the pitest results from Eclipse’s plugins folder while another test was running, making it difficult to compare all of the mutation results that were affected by recent modifications. If it was possible to test each function separately the mutation testing would have been less tedious to go through, and improve the speed at which correlations between code changes and mutation coverage results could be made.
