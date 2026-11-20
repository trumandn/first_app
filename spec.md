# Beam Designer

## Purpose

A beam design tool for engineers and students. The user sets up a beam,
places loads on it, and picks a steel or lumber section. The app shows the
shear, moment, and deflection diagrams and checks whether the beam is strong
and stiff enough, updating instantly as anything changes.

## Users

A civil engineering student checking a homework beam, or an engineer sizing
a floor beam or deck joist.

## Inputs
- Support type
  - Units: none.
  - Range: Simply supported or Cantilever (fixed at the left end).
  - Default: Simply supported.
  - Control: radio group or segmented control.
- Span
  - Units: ft.
  - Range: 4 to 40 ft.
  - Default: 20 ft.
  - Control: synchronized slider and number box.
- Point loads
  - Units: magnitude in kips; position in ft.
  - Range: magnitude accepts any finite numeric value, including negative
    values; position is from 0 to the span. There is no maximum load count
    and no maximum magnitude.
  - Default: no point loads are present; a newly added point load has a
    magnitude of 1 kip and a position at midspan, or at the free end for
    a cantilever.
  - Control: an editable load row with number boxes for magnitude and
    position, a remove button, and an "Add point load" button. The point
    load marker can also be dragged on the beam drawing, snapping to 0.25 ft.
- Uniform loads
  - Units: magnitude in kips/ft; start and end positions in ft.
  - Range: magnitude accepts any finite numeric value, including negative
    values; start and end positions are from 0 to the span. There is no
    maximum load count and no maximum magnitude.
  - Default: no uniform loads are present. A newly added uniform load has
    no magnitude and no interval until the user supplies both; it should not
    add a load to the calculations before both ends are valid.
  - Control: an editable load row with number boxes for magnitude, start,
    and end, a remove button, and an "Add uniform load" button.
- Material
  - Units: none.
  - Range: Steel or Douglas Fir-Larch No. 2.
  - Default: Steel.
  - Control: dropdown.
- Section
  - Units: section properties are in in^4, in^3, and lb/ft where applicable.
  - Range: the table options for the selected material.
  - Default: W10x12 for the default Steel material.
  - Control: separate dropdown, filtered to the selected material.
  - Available sections:

  Steel W-shapes (E = 29,000 ksi, allowable bending stress Fb = 30 ksi):

  | Section | Ix (in^4) | Sx (in^3) | Weight (lb/ft) |
  |---------|-----------|-----------|----------------|
  | W8x10   | 30.8      | 7.81      | 10 |
  | W10x12  | 53.8      | 10.9      | 12 |
  | W12x26  | 204       | 33.4      | 26 |
  | W14x22  | 199       | 29.0      | 22 |
  | W16x26  | 301       | 38.4      | 26 |
  | W18x35  | 510       | 57.6      | 35 |
  | W21x44  | 843       | 81.6      | 44 |
  | W24x55  | 1350      | 114       | 55 |

  Douglas Fir-Larch No. 2 lumber (E = 1,600 ksi, Fb = 0.900 ksi):

  | Section | Ix (in^4) | Sx (in^3) |
  |---------|-----------|-----------|
  | 2x8     | 47.63     | 13.14     |
  | 2x10    | 98.93     | 21.39     |
  | 2x12    | 177.98    | 31.64     |

- Deflection limit
  - Units: ratio of span to a number.
  - Range: L/240 or L/360.
  - Default: L/360.
  - Control: radio buttons.
- Cut position
  - Units: ft.
  - Range: 0 to the span.
  - Default: midspan.
  - Control: synchronized number box and draggable vertical line on the beam
    drawing. The line snaps to 0.25 ft, and arrow-key adjustment is also
    available.
- Cut side
  - Units: none.
  - Range: left or right.
  - Default: left.
  - Control: radio buttons or a segmented control.
- Reset
  - Units: none.
  - Range: no selectable values.
  - Default: restores the default input state.
  - Control: "Reset to example" button. The reset state is simply supported,
    20 ft span, no point loads, no uniform loads, Steel material, W10x12
    section, L/360 deflection limit, midspan cut position, and left cut side.
- Load validation
  - Point-load positions are clamped to 0 and the span.
  - Uniform-load start and end positions are clamped to 0 and the span. If
    the clamped start is greater than the clamped end, swap them.
  - Empty or nonnumeric fields are invalid and show an inline message with
    a red border.
  - If any input is invalid, calculations pause and the app retains the last
    valid result visibly marked "Last valid result." Restoring every field
    to a valid value resumes updates immediately.
- Self-weight is ignored.

## Calculations
- Sign conventions:
  - Downward point loads and downward-distributed loads are positive.
  - Shear V(x) is the sum of vertical forces to the left of x, with upward
    forces positive and downward forces negative. Equivalently,
    V = left reaction - downward loads to the left.
  - Moment is positive for sagging: compression on top and tension on
    bottom. On the simply supported beam, this produces positive moments
    under downward loads.
  - Downward deflection is positive.
- Reactions from statics.
- Shear V(x) and moment M(x) along the beam, at 1 ft spacing or finer,
  plus exactly at every load point and load start/end.
- At a point load or a discontinuity in shear, report the right-hand shear
  limit V(x+) in diagrams and check-card extrema. Moment remains continuous
  unless a concentrated couple is present, which is outside the scope.
- Diagrams show signed values. "Maximum" means the largest signed value and
  "minimum" means the smallest signed value. Labels identify the absolute
  maximum separately with its |value| and location. If positive and negative
  values tie in magnitude, show the positive-location value first.
- Deflection y(x) by integrating M/EI twice with the boundary conditions
  for the support type. For a simply supported beam, y = 0 at both ends.
  For a cantilever, the left end is fixed: y = 0 and slope = 0 at x = 0,
  and the right end is free.
  Integrate the sagging-positive moment with EI curvature so the resulting
  signed y is positive downward, or negate the conventional elastic-curve
  coordinate only after solving.
- Bending stress fb = |M max| x 12 / Sx (ksi). Bending check passes if
  fb <= Fb.
- Deflection check passes if |y max| <= L/240 or L/360 (L in inches).
- Lightest passing section: of the sections in the chosen material, the
  lightest steel section that passes both checks (if two weigh the same,
  the one with the larger Ix), or the first lumber size in the table
  that passes.
- Units: kips, ft, and kip-ft on screen; inches for deflection.
- Display precision:
  - Lengths and positions: 2 decimal places.
  - Loads and reactions: 2 decimal places.
  - Shear and moment: 2 decimal places.
  - Deflection: 3 decimal places.
  - Stress and allowable stress: 3 decimal places.

## Outputs
- A beam drawing with the supports, load arrows (point loads as single
  arrows, uniform loads as a row of small arrows), and the reactions.
  Reactions are labeled with their numerical value and unit. Simply
  supported beams show the vertical reactions at each support, with upward
  reactions shown as positive and downward reactions shown as negative.
  Cantilevers show the vertical reaction and fixed-end moment at the fixed
  left support. The fixed-end moment uses the sagging-positive moment
  convention and is shown with a direction arrow and its kip-ft value.
- Three diagrams stacked under the beam and lined up with it on the same
  x scale: shear (kips), moment (kip-ft), and deflected shape (in). Each
  diagram is filled and shows its maximum value labeled at its location.
- Moving the mouse over any diagram shows a vertical line across all three
  with the values of V, M, and y at that x.
- Two check cards: Bending and Deflection. Each shows the actual value, the
  allowable value, a bar for the ratio (green under 70%, amber 70-100%,
  red over 100%), and PASS or FAIL.
- A line suggesting the lightest passing section, with a button to use it.
- A section view for the internal forces at a user-selected point:
  - A cut-position control lets the user select a position along the beam,
    from 0 to the span. The numeric cut position and draggable cut line stay
    synchronized.
  - The cut position can be changed by dragging a vertical cut line on the
    beam drawing, snapping to 0.25 ft.
  - The user can choose to show and analyze the left or right portion of
    the beam after the cut.
  - The cut view uses the whole-beam sign conventions above: V is the
    right-hand limit V(x+) at the cut, and M is the moment at x. At a cut
    exactly at a point load, the view may show both V(x-) and V(x+) and
    the moment value. The side toggle changes the displayed retained portion
    only, not the cut-face sign convention.
  - The section view updates simultaneously with the cut position and side
    selection, and shows a cut-away image of the selected beam portion.
  - The image includes the retained end support or loads that remain on
    the selected portion and marks the cut face.
  - The cut face shows the internal vertical reaction force (shear) and
    internal moment. Reactions due to external supports and applied loads
    that act on the retained portion are also labeled.
- Everything updates immediately whenever any input changes.

## Look and feel
- Opens with an example already loaded (20 ft simply supported W12x26
  with a 1 kip/ft uniform load over the full span), so it is useful the
  moment it opens.
- Clean and professional, like a commercial engineering web app: light
  background, white cards with soft shadows, one accent color, a modern
  system font, generous spacing. No clutter.
- Inputs in a panel on the left; drawing, diagrams, and checks on the
  right. On a phone, the panels stack.
- Every input has a short label with units. Hovering over a check card
  explains the formula used.
- Controls are keyboard accessible, have visible keyboard focus, and have
  short accessible labels. All results and validation messages are exposed
  through appropriate text or assistive-technology status updates.
- The cut-position control has an equivalent text/number input and arrow-key
  support. The cut line can be adjusted without dragging. The selected
  position, side, shear, and moment are announced or available to
  assistive technology.
- On a phone, the section view follows the beam drawing and diagrams below
  the stacked input panel.

## Validation
1. Simply supported, 20 ft, W12x26, uniform 1 kip/ft over the full span:
   reactions 10.00 k and 10.00 k; max shear 10.00 k; max moment
   50.00 kip-ft at 10.00 ft; fb = 17.964 ksi; max deflection 0.609 in at
   10.00 ft; L/360 = 0.667 in; both checks pass.
2. Simply supported, 20 ft, W12x26, point load 10 k at 5 ft: reactions
   7.50 k (left) and 2.50 k (right); max moment 37.50 kip-ft at 5.00 ft;
   fb = 13.473 ksi; max deflection 0.340 in at 8.82 ft.
3. Cantilever, 10 ft, W12x26, point load 5 k at the free end: reaction
   5.00 k and fixed-end moment 50.00 kip-ft; max deflection 0.487 in at
   the free end (PL^3/3EI); L/360 = 0.333 in, so deflection fails.
4. Simply supported 12 ft 2x10 joist, uniform 0.05 kip/ft: max moment
   0.90 kip-ft; fb = 0.505 ksi; max deflection 0.147 in at 6.00 ft;
   both pass.

## Build stages

1. **Layout mockup**
   - Build the static desktop layout and responsive shell without real
     calculations.
   - Include the left input panel, beam and diagram output area, check cards,
     and cut-view placeholder using the default labels and sample values.
   - Test: Check desktop and phone layouts, panel stacking, control
     placement, label clarity, visual alignment, and obvious placeholders.

2. **Input state**
   - Add support, span, load rows, material/section, deflection limit, cut
     position, cut side, reset, and validation behavior.
   - Keep output values as mock data or placeholders.
   - Test: Check every default, synchronized span/cut controls, adding and
     removing loads, negative loads, clamping and swapping, invalid fields,
     reset behavior, and keyboard use.

3. **Beam and load geometry**
   - Render the beam, supports, point-load arrows, uniform-load arrows,
     reactions, and draggable cut line.
   - Test: Check simply supported and cantilever geometry, load arrows,
     reaction labels, cut snapping, control synchronization, and keyboard
     controls.

4. **Statics and diagrams**
   - Implement reactions, shear, moment, deflection, 1 ft or finer sampling,
     linked diagram hover, extrema labels, and check cards.
   - Test: Run all four validation cases and check sign conventions, units,
     display precision, PASS/FAIL ratios, and live updates.

5. **Section view**
   - Implement the live cut-away retained-beam view using the current shear
     and moment values.
   - Support left/right selection and exact point-load discontinuities.
   - Test: Check both retained sides, cut-face values, retained loads and
     supports, point-load discontinuities, and live cut updates.

6. **Section selection and polish**
   - Implement lightest passing section suggestions and "Use this section,"
     formula tooltips, mobile polish, accessibility, and final visual
     consistency.
   - Test: Check steel and lumber suggestions, no-passing-section behavior,
     formula tooltips, desktop and phone layouts, keyboard and screen-reader
     behavior, and reset to example.

## Out of scope
- Self-weight, shear stress, lateral-torsional buckling, load
  combinations, and continuous beams.
- Saving designs.