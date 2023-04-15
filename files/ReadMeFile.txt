The Maximal Tori of Finite Exceptional Groups of Lie Type

This is an accompanying file to the setup file. Here we have decriptions of all functions contained in the setup file and a few examples to demonstrate their use. 

------------------------------------------------------ makew ------------------------------------------------------------------------------------------------------------

The function makew enables the user to construct class representatives from the tables using roots of the root system. The class representative is returned either in a matrix or permutation representation of the Weyl group, depending on what the user specifies. The function takes as arguments: 

W : A string, that must be one of "G2", "F4", "E6", "E7", "E8", corresponding to which Weyl group the representative you desire lies inside. If an invalid string is given, 0 is returned. For the twisted groups, the classes are represented by elements that lie inside the non-twisted Weyl groups, and so if you wish to construct a class representative from a twisted Weyl group, W will be the string representing the non-twisted Weyl group.

R : An adjoint or simply connected root datum corresponding to the string W. If no isogeny type is specified when calling RootDatum(), the default type is adjoint.

L : A sequence or indexed set containing the integers corresponding to the root indexes for your required class representative.

type : Either "perm" or "mat", depending on if you want your representative to be given as a permutation or matrix respectively. If an invalid string is given, 0 is returned.

The function returns your desired class representative either as a permutation or matrix. If "mat" is given as the third argument, then returned is a matrix coerced into ReflectionGroup(W) where W is the string given as the first argument. If "perm" is given, then returned is a permutation coerced into a permutation representation of the desired Weyl group given by the command WeylGroup().


------------------------------------------------------------------------------------------------------------------------------------------------------------------




------------------------------------------------------ TwistCent ------------------------------------------------------------------------------------------------------------

The function TwistCent computes the sigma centraliser of a Weyl group element inside a matrix representation. The function takes as arguments:

R : An irreducible root datum indicating the untwisted type corresponding to the group of interest.

tw : Indicates the diagram automorphism associated to the group of interest; 0 for untwisted, 2 for the triality automorphism on D4, and 1 for each other twisted type.

w : The element we wish to construct the sigma centraliser of. Must be given as a matrix.

The function returns the sigma centraliser as a subgroup of the untwisted Weyl group inside the matrix representation. We remark that if tw is set to be 0, then TwistCent will simply compute the centraliser of w inside its parent Weyl group. The root datum R can be of either adjoint or simply connected type.


------------------------------------------------------------------------------------------------------------------------------------------------------------------




------------------------------------------------------ TwistClass ------------------------------------------------------------------------------------------------------------

The function TwistClass computes the sigma conjugacy class of a given element inside the untwisted Weyl group. The function takes as argument:

R : An irreducible root datum indicating the untwisted type corresponding to the group of interest.

tw : Indicates the diagram automorphism associated to the group of interest; 0 for untwisted, 2 for the triality automorphism on D4, and 1 for each other twisted type.

w : The element we wish to construct the sigma centraliser of. Must be given as a matrix.

The function returns the sigma-conjugacy class of w inside W as an indexed set. If C is the returned sigma-conjugacy class, individual elements of C can be accessed using C[i] for some integer i. If tw is set to be 0, this function simply computes the conjugacy class of w inside its parent Weyl group. The root datum R can be of either adjoint or simply connected type.

------------------------------------------------------------------------------------------------------------------------------------------------------------------



------------------------------------------------------ TorusStructure ------------------------------------------------------------------------------------------------------------

The function TorusStructure computes the structure of a maximal torus corresponding to a given element in a given finite group of Lie type defined over a field of order q. The function takes as argument: 

R : An irreducible root datum indicating the untwisted type corresponding to the group of interest.

tw : Indicates the diagram automorphism; 0 for untwisted, 2 for the triality automorphism on D4, and 1 for each other twisted type.

w :  An element of the Weyl group of R (in the standard matrix representation) corresponding to the torus of interest.

q :  The prime power associated to the group of interest.

The function outputs a sequence x containing the torsion coefficients of the maximal torus of interest. The structure of the maximal torus is then the direct product of the cyclic groups of order x[i], over i. The root datum R can be of either adjoint or simply connected type.

------------------------------------------------------------------------------------------------------------------------------------------------------------------ 



------------------------------------------------------ Examples ------------------------------------------------------------------------------------------------------------


We start by considering W(F4). We shall use makew to construct the class representative of class 4A inside a matrix representation and then make the matrix qw - Id. Moreover, we shall use row / column operations to diagonalise this matrix to show that the maximal torus 

> R := RootDatum("F4");			//Adjoint root datum of type F4
> w := makew("F4",R,[1,4,13,14],"mat");  //Construct class representative using makew
> w;                   
[-1 -2 -4 -2]
[ 0  1  2  2]
[ 1  1  1  0]
[-1 -2 -2 -1]
> Z<q> := PolynomialRing(Integers(), 1);	 //Polynomial ring over the integers
> M := MatrixAlgebra(Z,4);  				//Matrix algebra of 4x4 matrices defined over the polynomial ring
> k := q*(M!w)-1; 							//Polynomial matrix q.w-Id
> k;
[-q - 1   -2*q   -4*q   -2*q]
[     0  q - 1    2*q    2*q]
[     q      q  q - 1      0]
[    -q   -2*q   -2*q -q - 1]
> AddRow(~k,1,3,1); AddRow(~k,q,1,3); AddRow(~k,-q,1,4); AddColumn(~k,-q,1,2); AddColumn(~k,-3*q-1,1,3); AddColumn(~k,-2*q,1,4); k; //Row/col operations to diagonalize
[           -1             0             0             0]
[            0         q - 1           2*q           2*q]
[            0      -q^2 + q    -3*q^2 - 1        -2*q^2]
[            0     q^2 - 2*q     3*q^2 - q 2*q^2 - q - 1]
> AddRow(~k,q,2,3); AddRow(~k,-q,2,4); AddRow(~k,1,4,2); AddRow(~k,-q,2,4); AddColumn(~k,q^2+q,2,3); AddColumn(~k,q-1,2,4); k; //Row/col operations
[      -1        0        0        0]
[       0       -1        0        0]
[       0        0 -q^2 - 1        0]
[       0        0 -q^3 - q -q^2 - 1]
> AddRow(~k,-q,3,4); k[3] := -k[3]; k[4] := -k[4]; k; //Row/col operations, now k is diagonal.
[     -1       0       0       0]
[      0      -1       0       0]
[      0       0 q^2 + 1       0]
[      0       0       0 q^2 + 1]

Hence we see that Tw ~ (q^2+1)^2 as expected.


The following example demonstrates how to use the function TorusStructure to obtain the structures of maximal tori inside the twisted group ^2G_2(27). In this case our diagram automorphism is involutive, so we give the second argument of TorusStructure as 1.

> R:=RootDatum("G2");			//Adjoint and simply connected root datum of type G2
> w:=makew("G2",R,[1],"mat");    //Class representative                 
> TorusStructure(R,1,w,27);
[ 19 ]
> w:=makew("G2",R,[6,2,1],"mat");
> TorusStructure(R,1,w,27);
[ 2, 14 ]
> w:=makew("G2",R,[6],"mat");
> TorusStructure(R,1,w,27);
[ 37 ]
> w:=makew("G2",R,[],"mat");
> TorusStructure(R,1,w,27);
[ 26 ]

We conclude that the group ^2G_2(27) has cyclic maximal tori of orders 19,26 and 37, and a torus isomorphic to the direct product of the group of order 2 and the abelian group of order 14.
One can check this against Table 9 in the associated paper.

The following example demonstrates use of the functions TwistClass and TorusStructure to obtain the twisted conjugacy classes and maximal torus structures inside the Suzuki group ^2B_2(8).
Again, we give the second argument for TwistClass and TorusStructure as 1. 

> R:=RootDatum("B2");      //Adjoint root datum of type B2
> W:=ReflectionGroup(R); 	//Corresponding Weyl group
> TCs:={};
> SoFar:={};
> for g in W do
for> if not g in SoFar then		//if we haven't considered element g yet
for|if> TC:=TwistClass(R,1,g); //sigma-class of Weyl group element
for|if> Include(~TCs,TC);		
for|if> SoFar:=SoFar join TC;		//keep track of which elements we have considered 
for|if> end if;
for> end for;
> for g in TCs do
for> TorusStructure(R,1,Random(g),8);  //calculate the maximal torus structure corresponding to each twist class
for> end for;
[ 13 ]
[ 7 ]
[ 5 ]

We conclude that the group ^2B_2(8) has cyclic maximal tori of orders 5,7 and 13.

The following example demonstrates use of the functions TwistClass and TorusStructure o obtain the twisted conjugacy classes and maximal torus structures inside the group ^3D_4(3)
Here we give the second argument of TwistClass and TorusStructure as 2, to indicate the triality diagram automorphism. The same method described above is employed here.

> R:=RootDatum("D4");
> W:=ReflectionGroup(R);
> TCs:={};
> SoFar:={};
> for g in W do
for> if not g in SoFar then
for|if> TC:=TwistClass(R,2,g);
for|if> Include(~TCs,TC);
for|if> SoFar:=SoFar join TC;
for|if> end if;
for> end for;
> for g in TCs do
for> TorusStructure(R,2,Random(g),3);
for> end for;
[ 104 ]
[ 2, 26 ]
[ 7, 7 ]
[ 4, 28 ]
[ 73 ]
[ 56 ]
[ 13, 13 ]

Again, we may read off unique structures of the maximal tori, as direct products of cyclic groups.

We are not currently aware of the existence of any in-built MAGMA function which a user can employ to obtain maximal torus structures in twisted groups.


The following example uses makew and TorusStructure to show that the structure of maximal tori in both the adjoint and simply connected Weyl groups coincide if they correspond to the same product of reflection matrices. We consider the class 4I in W(E7) and work over the field GF(5).


> Rad := RootDatum("E6" : Isogeny := "Ad"); 	//Adjoint root datum of type E6
> Rsc := RootDatum("E6" : Isogeny := "SC");		//Simply connected root datum of type E6
> wad := makew("E6",Rad,[1,4,6,5],"mat");		//Construct class representative in the adjoint case
> wsc := makew("E6",Rsc,[1,4,6,5],"mat");		//Construct class representative in the simply connected case
> TorusStructure(Rad,0,wad,5);			//Structure of corresponding maximal torus
[ 24, 624 ]
> TorusStructure(Rsc,0,wsc,5);
[ 24, 624 ]

Hence we see that the structure of the maximal torus in both the adjoint and simply connected case is the same.

As demonstrated in the associated paper (and the above example), the maximal tori of adjoint and simply connected groups of exceptional type agree. The following examples demonstrate that, in general, it is not necessarily the case that the adjoint and simply connected groups of a given type have the same maximal tori.

> R:=RootDatum("B3");                             // root datum associated to the special orthogonalgroup SO_7(3)
> R1:=RootDatum("B3" : Isogeny:="SC");            // root datum associated to the spin group Spin_7(3)
> W:=ReflectionGroup(R);                        
> W1:=ReflectionGroup(R1); 
> C:=ConjugacyClasses(W);                        // groups of interest are untwisted, so maximal tori correspond to conjugacy classes in the Weyl group
> C1:=ConjugacyClasses(W1);                                 
> #C;
10
> Tori:=[];
> Tori1:=[];
> for i:=1 to 10 do
for> Append(~Tori,TorusStructure(R,0,C[i][3],3));
for> Append(~Tori1,TorusStructure(R1,0,C1[i][3],3));
for> end for;
> Tori;
[
    [ 2, 2, 2 ],
    [ 4, 4, 4 ],
    [ 2, 4, 4 ],
    [ 2, 2, 4 ],
    [ 2, 8 ],
    [ 4, 8 ],
    [ 26 ],
    [ 2, 20 ],
    [ 2, 10 ],
    [ 28 ]
]
> Tori1;
[
    [ 2, 2, 2 ],
    [ 4, 4, 4 ],
    [ 2, 8 ],
    [ 4, 8 ],
    [ 4, 8 ],
    [ 2, 8 ],
    [ 26 ],
    [ 40 ],
    [ 20 ],
    [ 28 ]
]

We see that, for example, Spin_7(3) has a cyclic maximal torus of order 40, while no such torus exists in SO_7(3).

> R:=RootDatum("C4");                                    // root datum associated to the projective conformal symplectic group PCSp_8(3)
> R1:=RootDatum("C4" : Isogeny:="SC");                   // root datum associated to the symplectic group Sp_8(3)
> W:=ReflectionGroup(R);
> W1:=ReflectionGroup(R1);
> C:=ConjugacyClasses(W);                                // groups of interest are untwisted, so maximal tori correspond to conjugacy classes in the Weyl group
> C1:=ConjugacyClasses(W1);
> for i:=1 to 20 do
for> TorusStructure(R,0,C[i][3],3);
for> end for;
[ 2, 2, 2, 2 ]
[ 4, 4, 4, 4 ]
[ 2, 2, 8 ]
[ 4, 4, 8 ]
[ 2, 4, 8 ]
[ 2, 4, 8 ]
[ 2, 2, 8 ]
[ 4, 4, 8 ]
[ 8, 8 ]
[ 2, 26 ]
[ 10, 10 ]
[ 2, 20 ]
[ 4, 40 ]
[ 2, 40 ]
[ 2, 40 ]
[ 2, 40 ]
[ 4, 28 ]
[ 104 ]
[ 56 ]
[ 82 ]
> for i:=1 to 20 do
for> TorusStructure(R1,0,C1[i][3],3);
for> end for;
[ 2, 2, 2, 2 ]
[ 4, 4, 4, 4 ]
[ 2, 2, 2, 4 ]
[ 2, 4, 4, 4 ]
[ 2, 2, 4, 4 ]
[ 4, 4, 8 ]
[ 8, 8 ]
[ 2, 2, 8 ]
[ 2, 4, 8 ]
[ 2, 26 ]
[ 2, 4, 20 ]
[ 2, 2, 10 ]
[ 10, 10 ]
[ 2, 40 ]
[ 2, 2, 20 ]
[ 80 ]
[ 2, 28 ]
[ 2, 52 ]
[ 4, 28 ]
[ 82 ]
 
So that the projective conformal symplectic group PCSp_8(3) has cyclic maximal tori of order 104, while the symplectic group Sp_8(3) has no torus with this structure.  


