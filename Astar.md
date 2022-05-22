/*java */
import java.util.ArrayList;
import java.util.Comparator;
import java.util.*;
class AStar{

   //main func
        public static void main(String[] args)  {
            graph graph = new graph();
            graph.setGraph();
            state goal = graph.AStar();
            graph.getPath(goal);
        }
 
    
}

 class state {   // str NODE
    
    String name;
    boolean visited = false;
    boolean isGoal = false;
    int weight ;
    state father;
    int h ;
    ArrayList<state> children = new ArrayList<state>();
    ArrayList<Integer> costs = new ArrayList<Integer>();

    state(String s){
        name = s;
    }

    state getCopy(){
        state s = new state(name);
        s.visited = visited;
        s.isGoal = isGoal;
        s.weight = weight;
        s.h = h;
        s.father = father;
        for(int i=0 ;i<children.size();i++){
            s.addChild(children.get(i).getCopy(),costs.get(i));
        }
        return s;
    }

    void addChild(state s , int cost){ //bulid new edge
        children.add(s);
        costs.add(cost);
    }

    boolean same(state s){
        if(s.name == this.name){
            return true;
        }
        return false;
    }

    boolean inArray(ArrayList<state> arrayList){
        for(int i=0 ;i<arrayList.size();i++){
            if(same(arrayList.get(i))){
                return true;
            }
        }
        return false;
    }
}
  
 class Comp implements Comparator<state> {
    public int compare(state s1 , state s2){
        int tmpN1 = s1.weight;
        int tmpN2 = s2.weight;
        if(tmpN1>tmpN2){
            return 1;
        }
        else if(tmpN2<tmpN1){
            return -1;
        }
        else {
            return 0;
        }
    }
}



 class graph {
state start ;


state AStar(){
    Queue<state> open = new PriorityQueue<state>(new Comp());
    ArrayList<state> close = new ArrayList<state>();
    open.add(start);
    boolean success = false;
    state tmp ;

    while (!open.isEmpty() && !success){
        System.out.println("ss");
        tmp = open.poll();
        if(tmp.isGoal){
            success = true;
            return tmp;
        }
        else{
            close.add(tmp.getCopy());
            for(int i=0 ; i<tmp.children.size() ;i++){
                if(!tmp.children.get(i).inArray(close)){
                    tmp.children.get(i).father = tmp.getCopy();
                    tmp.children.get(i).weight = tmp.weight + tmp.costs.get(i) + tmp.children.get(i).h;
                    open.add(tmp.children.get(i).getCopy());
                    tmp.children.get(i).weight -= tmp.children.get(i).h;
                }
            }
        }
    }
    return null;
}

void getPath(state s){
    state tmp = s;
    ArrayList<String> path = new ArrayList<>();
    while (s!=null){
        path.add(s.name);
        s = s.father;
    }
    for(int i=path.size()-1 ; i>=0 ; i--){
        System.out.println(path.get(i));
    }
}


void setGraph(){

    state S = new state("S");
    state A = new state("A");
    state B = new state("B");
    state C = new state("C");
    state G = new state("G");
    G.isGoal = true;

    S.addChild(A,1);
    S.addChild(B,4);
    A.addChild(B,3);
    A.addChild(C,6);
    A.addChild(G,1);
    B.addChild(C,8);
    C.addChild(G,10);

    S.h = 7;
    A.h = 6;  
    B.h = 4;
    C.h = 2;
    G.h = 0;

    start = S.getCopy();
    }

}
