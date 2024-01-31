# Packed vs Unpacked

> [!NOTE] IEEE 1800-2017 7.4 Packed and unpacked arrays
> The term packed array is used to refer to the dimensions declared before the data identifier name. The term unpacked array is used to refer to the dimensions declared after the data identifier name.

```Verilog
bit [7:0] some_var; // packed dimension
bit another_var[7:0]; // unpacked dimension
```

 - A packed array is guaranteed to be represented as a contiguous set of bits while packed array may or not be so.
 - Packed arrays allow arbitrary length integer types. And not every data type is packable.
 - Integer types with predefined widths shall not have packed array dimensions declared. These types are `byte`, `shortint`, `int`, `longint`, `integer`, and `time`.
 ```Verilog
 byte c2; // same as bit signed [7:0] c2; 
 integer i1; // same as logic signed [31:0] i1;
 ```
-  Unpacked arrays may be fixed-size arrays, dynamic arrays, associative arrays or queues. And can be made up of any data type.
 [Useful Link](https://verificationacademy.com/forums/ovm/difference-between-packed-and-unpacked-arrays)

## Array Part Select

```Verilog
logic [31: 0] a_vect;
logic [0 :31] b_vect;
logic [63: 0] dword;
integer sel;

// ▪ Base expression (variable)
// ▪ Width expression (constant)
// ▪ Offset direction:
//     o Positive +:
//     o Negative -:
//[base_expr +: width_expr]
//[base_expr -: width_expr]

a_vect[ 0 +: 8] // == a_vect[ 7 : 0]
a_vect[15 -: 8] // == a_vect[15 : 8]
b_vect[ 0 +: 8] // == b_vect[0 : 7]
b_vect[15 -: 8] // == b_vect[8 :15]
dword[8*sel +: 8] // variable part-select with fixed width
```

## Net Declaration Assignment
[TODO]
*References* 
* 10.3.1 in IEEE 1800-2017

## Run-time vs Elaboration-time Constant
[TODO]
*References* 
* 6.20 and 6.20.6 in IEEE 1800-2017

## Constant Initialization

```Verilog
module tb;
  
  parameter some_param = 33; // known during compilation/elaboration
  const int some_var = 22;   // initialized in run time
  
  const struct {
    int abc = some_param;
    int cde = some_var; // ERROR
    int fgh = abc;      // ERROR
  } const_struct;
  
  struct {
    const int abc = 10; // ERROR, can't have const qualifier
    int cde = 0;
  } const_member;
  
  initial begin
    const_struct.cde = 10; // ERROR, can't assign const variable
  end
  
endmodule
```

## Membership Operator
[TODO]

## Interface Classes
[TODO]

## Package Nesting


> [!Quote] Dave Rich
> SystemVerilog does not allow nesting of package declarations. The best thing for you do do is to define a file that is a list of package import statements and have users `` `include `` that file where needed.
[Link](https://stackoverflow.com/a/40007155/12414944)


> [!NOTE] IEEE 1800-2017 26.6 Exporting imported names from packages
> By default, declarations imported into a package are not visible by way of subsequent imports of that package. Package export declarations allow a package to specify that imported declarations are to be made visible in subsequent imports. A package export may precede a corresponding package import 



# SVA

## Implication, Antecedent and Consequent

Implication operator ties the antecedent and consequent. If antecedent(left-hand operand) holds true it implies that the consequent(right-hand operand) should hold true.
```Verilog
property some_prop;
	antecedent_expr |-> consequent_expr;
endproperty
```

There are two forms of implication operator. Overlapping means that consequent should match at the same clocking event that `antecedent_expr` is matched. Non-overlapping implication adds one clock tick before `consequent_expr`, i.e one tick after end point of the `antecedent` match.

```Verilog
|-> # Overlapping implication
|=> # Non-overlapping implication
```

Non-overlapping implication can be expressed using overlapping operator plus 1 cycle delay.

```Verilog
... |=> consequent_expr;

# Non-Overlapping equivalent
... |-> ##1 consequent_expr;
```

*References* 
* Ashok B. Mehta, SystemVerilog Assertions and Functional Coverage.
* IEEE 1800-2017

## Vacuous Pass

### Boolean Implies operator


## Unbounded time delay in Antecedent


## Triggered and Matched Methods

