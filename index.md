go:-solution(S).

solution(ActionList):-init(I),nl,write(init),tab(4),write(I),
                      nl,solution(I,[],ActionList),!.

solution(State,VisitedStates,[]) :- final(State).

solution(State,VisitedStates, [Action|Rest]) :- applicable(Action,State) ,
                                        apply(Action,State,NewState),
                    \+visited(NewState,VisitedStates), write(Action), tab(4) , write(NewState), nl,
                    solution(NewState,[State|VisitedStates],Rest).

visited(State,[VisitedState|OtherVisitedStates]) :-   sameState(State,VisitedState).

visited(State,[VisitedState|OtherVisitedStates]) :- visited(State,OtherVisitedStates).
sameState(S,S).


init(state(0,0)).
final(state(2,X)).
applicable(emptyA,state(Qa,Qb)) :- Qa > 0.
applicable(emptyB,state(Qa,Qb)) :- Qb > 0.
applicable(fillA,state(Qa,Qb)) :- Qa < 4.
applicable(fillB,state(Qa,Qb)) :- Qb < 3.
applicable(emptyAinB,state(Qa,Qb)) :- Qa > 0 , Qtot is Qa + Qb , Qtot =< 3.
applicable(emptyBinA,state(Qa,Qb)) :- Qb > 0, Qtot is Qa + Qb , Qtot =< 4.
applicable(fillAwithB,state(Qa,Qb)) :- Qa < 4, Qtrasf is 4 - Qa , Qtrasf =< Qb.
applicable(fillBwithA,state(Qa,Qb)) :- Qb < 3, Qtrasf is 3 - Qb , Qtrasf=<Qa.

apply(emptyA,state(Qa,Qb),state(0,Qb)).
apply(emptyB,state(Qa,Qb),state(Qa,0)).
apply(fillA,state(Qa,Qb),state(4,Qb)).
apply(fillB,state(Qa,Qb),state(Qa,3)).
apply(emptyAinB,state(Qa,Qb),state(0,Qtot)) :- Qtot is Qa+Qb.
apply(emptyBinA,state(Qa,Qb),state(Qtot,0)) :- Qtot is Qa+Qb.
apply(fillAwithB,state(Qa,Qb),state(4,NewQb)) :- NewQb is Qb-(4-Qa).
apply(fillAwithB,state(Qa,Qb),state(NewQa,3)) :- NewQa is Qa-(3-Qb).
