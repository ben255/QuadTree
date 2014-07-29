package QuadTree;

public class QuadTree<Key extends Comparable> {
	Node root;
	int xMin,yMin,xMax,yMax;
	boolean init = false;
	FindQuadrantMid find;
	class Leaf extends Node{
		Key x, y;
		Object data;
		int[] value = new int[4];
		PartitionRect rect = new PartitionRect();
		Leaf(Key x, Key y, Object data){
			this.x = x;
			this.y = y;
			this.data = data;
		}
		void setDefaultPartition(int xMin, int yMin, int xMax, int yMax){
			rect.setValue(xMin, yMin, xMax, yMax);
			value = rect.getValue();
		}
		void setPartition(int xMin, int yMin, int xMax, int yMax, int direction){
			rect.setValue(xMin, yMin, xMax, yMax);
			rect.interval(direction);
			value = rect.getValue();
		}		
	}//endclass Leaf
	class Internal extends Node{
		Node NW,SW,NE,SE;
		int[] value = new int[4];
		PartitionRect rect = new PartitionRect();
		Internal(){
		}
		void setDefaultPartition(int xMin, int yMin, int xMax, int yMax){
			rect.setValue(xMin, yMin, xMax, yMax);
			value = rect.getValue();
		}
		void setPartition(int xMin, int yMin, int xMax, int yMax, int direction){
			rect.setValue(xMin, yMin, xMax, yMax);
			rect.interval(direction);
			value = rect.getValue();
		}
	}//endclass Internal
	class Node{
		Node(){
		}
	}//endclass Node
	QuadTree(int xMin,int yMin, int xMax, int yMax){
		this.xMin = xMin;
		this.yMin = yMin;
		this.xMax = xMax;
		this.yMax = yMax;
		find = new FindQuadrantMid();
	}
	public void insert(Key x, Key y, int data){
		root = insert(root,x,y,data, null);
	}
	private Node insert(Node h, Key x, Key y, Object data, int[] value){
		if(h == null){
			Leaf tempLeaf = new Leaf(x,y,data);
			if(!init){
				tempLeaf.setDefaultPartition(xMin, yMin, xMax, yMax);
				h = tempLeaf;
				init = true;
			}
			else{
				tempLeaf.setPartition(value[0], value[1], value[2], value[3],getDirection(tempLeaf.x,tempLeaf.y,value));
				h = tempLeaf;
			}
			if(value != null)
				System.out.println("ADDEDLEAF {x,y,data} {"+x+","+y+","+data+"}"+" x{"+value[0]+","+value[2]+"} y{"+value[1]+","+value[3]+"}");
			else
				System.out.println("IsNull");
		}
		else{
			if(h.getClass().equals(Leaf.class)){
				//need to split
				System.out.println("IsSplit");
				Leaf tempLeaf = (Leaf) h;
				Internal tempInternal = new Internal();
				tempInternal.setDefaultPartition(tempLeaf.value[0], tempLeaf.value[1], tempLeaf.value[2], tempLeaf.value[3]);
				h = tempInternal;
				
				//System.out.println("------------------- tempLeafx,y,data {"+tempLeaf.x+","+tempLeaf.y+","+tempLeaf.data+"} LeafsQuadrant: x{"
				//+tempInternal.value[0]+","+tempInternal.value[2]+"} y{"+
				//		tempInternal.value[1]+","+tempInternal.value[3]+"}");
				//System.out.println(tempLeaf.value[0]+","+tempLeaf.value[2]+"...."+tempLeaf.value[1]+","+tempLeaf.value[3]);
				
				h = insert(h,tempLeaf.x,tempLeaf.y, tempLeaf.data, tempInternal.value);
				
				//System.out.println("*******************  xydata {"+
				//		x+","+y+","+data+"}  LeafsQuadrant: x{"+tempInternal.value[0]+","+tempInternal.value[2]+"} y{"+
				//		tempInternal.value[1]+","+tempInternal.value[3]+"}");
				//System.out.println("value2 "+tempLeaf.value[0]+","+tempLeaf.value[2]+"...."+tempLeaf.value[1]+","+tempLeaf.value[3]);
				h = insert(h,x,y,data,value);
			}
			else{
				if(h.getClass().equals(Internal.class)){
					//is split just add leaf
					Internal tempInternal = (Internal) h;
					find.setValue(tempInternal.value[0], tempInternal.value[1], tempInternal.value[2], tempInternal.value[3]);
					int midX = find.getMidX();
					int midY = find.getMidY();
					System.out.println("IsNode....Searching MidPoint{x,y}: {"+midX+","+midY+"}");
					//add node in approiet partition
					if(less(x,(Key)(Integer)midX) && less(y,(Key)(Integer)midY)){
						System.out.println("SET LEAF:{x,y,data} {"+x+","+y+","+data+"} IN FKING NW...In Quadrant: "+
					"x{"+tempInternal.value[0]+","+tempInternal.value[2]+"} y{"+tempInternal.value[1]+","+tempInternal.value[3]+"}");
						tempInternal.NW = insert(tempInternal.NW ,x,y,data,tempInternal.value);
						h = tempInternal;
					}
					else if(less(x,(Key)(Integer)midX) && !less(y,(Key)(Integer)midY)){
						System.out.println("SET LEAF:{x,y,data} {"+x+","+y+","+data+"} IN FKING SW...In Quadrant: "+
					"x{"+tempInternal.value[0]+","+tempInternal.value[2]+"} y{"+tempInternal.value[1]+","+tempInternal.value[3]+"}");
						tempInternal.SW = insert(tempInternal.SW,x,y,data,tempInternal.value);
						h = tempInternal;
					}
					else if(!less(x,(Key)(Integer)midX) && less(y,(Key)(Integer)midY)){
						System.out.println("SET LEAF:{x,y,data} {"+x+","+y+","+data+"} IN FKING NE...In Quadrant: "+
					"x{"+tempInternal.value[0]+","+tempInternal.value[2]+"} y{"+tempInternal.value[1]+","+tempInternal.value[3]+"}");
						tempInternal.NE = insert(tempInternal.NE,x,y,data,tempInternal.value);
						h = tempInternal;
					}
					else if(!less(x,(Key)(Integer)midX) && !less(y,(Key)(Integer)midY)){
						System.out.println("SET LEAF:{x,y,data} {"+x+","+y+","+data+"} IN FKING SE...In Quadrant: "+
					"x{"+tempInternal.value[0]+","+tempInternal.value[2]+"} y{"+tempInternal.value[1]+","+tempInternal.value[3]+"}");
						tempInternal.SE = insert(tempInternal.SE,x,y,data,tempInternal.value);
						h = tempInternal;
					}
				}
			}
		}
		/*if(h.getClass().equals(Leaf.class)){
			Leaf tempLeaf = (Leaf) h;
			System.out.println("RETURNING Leaf {x,y,data} {"+x+","+y+","+data+"}"+
					" x{"+tempLeaf.value[0]+","+tempLeaf.value[2]+"} y{"+tempLeaf.value[1]+","+tempLeaf.value[3]+"}");
		}
		else{
			Internal tempInternal = (Internal) h;
			System.out.println("RETURNING Internal {x,y,data} {"+x+","+y+","+data+"}"+
					" x{"+tempInternal.value[0]+","+tempInternal.value[2]+"} y{"+tempInternal.value[1]+","+tempInternal.value[3]+"}");
		}*/
		return h;
	}//ENDING INSERT***************************************************************************************
	//STARTING SEARCH**************************************************************************************'
	boolean findPoint(Key x, Key y){
		Node temp = searchTree(root,x,y);
		Leaf leaf = popLeaf();
		if(leaf != null){
			System.out.println("Found leaf:{x,y,data}: {"+leaf.x+","+leaf.y+","+leaf.data+"} LeafQuadrant: x{"+
					leaf.value[0]+","+leaf.value[2]+"} y{"+leaf.value[1]+","+leaf.value[3]+"}");
			return true;
		}else
			System.out.println("Didnt find leaf");
		return false;
	}
	Leaf savedLeaf;
	void pushLeaf(Leaf leaf){
		savedLeaf = leaf;
	}
	Leaf popLeaf(){
		Leaf leaf = savedLeaf;
		savedLeaf = null;
		return leaf;
	}
	
	Node searchTree(Node h, Key x, Key y){
		if(h == null){
			return h;
		}else
			if(h.getClass().equals(Leaf.class)){
				Leaf temp = (Leaf) h;
				if(x == temp.x && y == temp.y)
					pushLeaf(temp);
				return h;
			}else{
				Internal tempInternal = (Internal) h;
				System.out.println("Searching Internal: x{"+tempInternal.value[0]+","+tempInternal.value[2]+"} y{"+
				tempInternal.value[1]+","+tempInternal.value[3]+"}");
				find.setValue(tempInternal.value[0], tempInternal.value[1], tempInternal.value[2], tempInternal.value[3]);
				int midX = find.getMidX();
				int midY = find.getMidY();
				if(less(x,(Key)(Integer)midX) && less(y,(Key)(Integer)midY)){
					tempInternal.NW = searchTree(tempInternal.NW ,x,y);
					h = tempInternal;
				}
				else if(less(x,(Key)(Integer)midX) && !less(y,(Key)(Integer)midY)){
					tempInternal.SW = searchTree(tempInternal.SW,x,y);
					h = tempInternal;
				}
				else if(!less(x,(Key)(Integer)midX) && less(y,(Key)(Integer)midY)){
					tempInternal.NE = searchTree(tempInternal.NE,x,y);
					h = tempInternal;
				}
				else if(!less(x,(Key)(Integer)midX) && !less(y,(Key)(Integer)midY)){
					tempInternal.SE = searchTree(tempInternal.SE,x,y);
					h = tempInternal;
				}
			}
		return h;
	}//ENDING SEARCH*****************************************************************************
	//DELETE OBJECT
	void deleteLeaf(Key x, Key y){
		root = deleteLeaf(root,x,y);
	}
	Node deleteLeaf(Node h, Key x, Key y){
		if(h == null){
			return h;
		}else{
			Internal tempInternal = (Internal) h;
			System.out.println("Searching Internal: x{"+tempInternal.value[0]+","+tempInternal.value[2]+"} y{"+
			tempInternal.value[1]+","+tempInternal.value[3]+"}");
			find.setValue(tempInternal.value[0], tempInternal.value[1], tempInternal.value[2], tempInternal.value[3]);
			int midX = find.getMidX();
			int midY = find.getMidY();
			if(less(x,(Key)(Integer)midX) && less(y,(Key)(Integer)midY)){
				if(!tempInternal.NW.getClass().equals(Leaf.class))
					tempInternal.NW = deleteLeaf(tempInternal.NW ,x,y);
				else{
					int[] checkQuad = new int[3];
					int check = 0;
					if(tempInternal.SW != null){
						if(tempInternal.SW.getClass().equals(Leaf.class))
							checkQuad[0] = 1;
						else
							checkQuad[0] = 0;
					}
					else
						checkQuad[0] = 0;
					if(tempInternal.NE != null){
						if(tempInternal.NE.getClass().equals(Leaf.class))
							checkQuad[1] = 1;
						else
							checkQuad[1] = 0;
					}
					else
						checkQuad[1] = 0;
					if(tempInternal.SE != null){
						if(tempInternal.SE.getClass().equals(Leaf.class))
							checkQuad[2] = 1;
						else 
							checkQuad[2] = 0;
					}
					else
						checkQuad[2] = 0;
					check = checkQuad[0] + checkQuad[1] + checkQuad[2];
					if(check > 1 || check == 0)
						tempInternal.NW = null;
					else if(checkQuad[0] == 1){
						Leaf temp = (Leaf) tempInternal.SW;
						temp.value = tempInternal.value;
						return h = temp;
					}
					else if(checkQuad[1] == 1){
						Leaf temp = (Leaf) tempInternal.NE;
						temp.value = tempInternal.value;
						return h = temp;
					}
					else if(checkQuad[2] == 1){
						Leaf temp = (Leaf) tempInternal.SE;
						temp.value = tempInternal.value;
						return h = temp;
					}
				}
				h = tempInternal;
			}
			else if(less(x,(Key)(Integer)midX) && !less(y,(Key)(Integer)midY)){
				if(!tempInternal.SW.getClass().equals(Leaf.class))
					tempInternal.SW = deleteLeaf(tempInternal.SW,x,y);
				else{
					int[] checkQuad = new int[3];
					int check = 0;
					if(tempInternal.NW != null){
						if(tempInternal.NW.getClass().equals(Leaf.class))
							checkQuad[0] = 1;
						else
							checkQuad[0] = 0;
					}
					else
						checkQuad[0] = 0;
					if(tempInternal.NE != null){
						if(tempInternal.NE.getClass().equals(Leaf.class))
							checkQuad[1] = 1;
						else
							checkQuad[1] = 0;
					}
					else
						checkQuad[1] = 0;
					if(tempInternal.SE != null){
					if(tempInternal.SE.getClass().equals(Leaf.class))
						checkQuad[2] = 1;
					else 
						checkQuad[2] = 0;
					}
					else
						checkQuad[2] = 0;
					check = checkQuad[0] + checkQuad[1] + checkQuad[2];
					if(check > 1 || check == 0)
						tempInternal.SW = null;
					else if(checkQuad[0] == 1){
						Leaf temp = (Leaf) tempInternal.NW;
						temp.value = tempInternal.value;
						return h = temp;
					}
					else if(checkQuad[1] == 1){
						Leaf temp = (Leaf) tempInternal.NE;
						temp.value = tempInternal.value;
						return h = temp;
					}
					else if(checkQuad[2] == 1){
						Leaf temp = (Leaf) tempInternal.SE;
						temp.value = tempInternal.value;
						return h = temp;
					}
				
					
				}
				h = tempInternal;
			}
			else if(!less(x,(Key)(Integer)midX) && less(y,(Key)(Integer)midY)){
				if(!tempInternal.NE.getClass().equals(Leaf.class))
					tempInternal.NE = deleteLeaf(tempInternal.NE,x,y);
				else{

					int[] checkQuad = new int[3];
					int check = 0;
					if(tempInternal.SW != null){
						if(tempInternal.SW.getClass().equals(Leaf.class))
							checkQuad[0] = 1;
						else
							checkQuad[0] = 0;
					}
					else
						checkQuad[0] = 0;
					if(tempInternal.NW != null){
						if(tempInternal.NW.getClass().equals(Leaf.class))
							checkQuad[1] = 1;
						else
							checkQuad[1] = 0;
					}
					else
						checkQuad[1] = 0;
					if(tempInternal.SE != null){
						if(tempInternal.SE.getClass().equals(Leaf.class))
							checkQuad[2] = 1;
						else 
							checkQuad[2] = 0;
					}
					else
						checkQuad[2] = 0;
					check = checkQuad[0] + checkQuad[1] + checkQuad[2];
					if(check > 1 || check == 0)
						tempInternal.NE = null;
					else if(checkQuad[0] == 1){
						Leaf temp = (Leaf) tempInternal.SW;
						temp.value = tempInternal.value;
						return h = temp;
					}
					else if(checkQuad[1] == 1){
						Leaf temp = (Leaf) tempInternal.NW;
						temp.value = tempInternal.value;
						return h = temp;
					}
					else if(checkQuad[2] == 1){
						Leaf temp = (Leaf) tempInternal.SE;
						temp.value = tempInternal.value;
						return h = temp;
					}
				}
				h = tempInternal;
			}
			else if(!less(x,(Key)(Integer)midX) && !less(y,(Key)(Integer)midY)){
				if(!tempInternal.SE.getClass().equals(Leaf.class))
					tempInternal.SE = deleteLeaf(tempInternal.SE,x,y);
				else{
					int[] checkQuad = new int[3];
					int check = 0;
					if(tempInternal.SW != null){
						if(tempInternal.SW.getClass().equals(Leaf.class))
							checkQuad[0] = 1;
						else
							checkQuad[0] = 0;
					}
					else
						checkQuad[0] = 0;
					if(tempInternal.NE != null){
						if(tempInternal.NE.getClass().equals(Leaf.class))
							checkQuad[1] = 1;
						else
							checkQuad[1] = 0;
					}
					else
						checkQuad[1] = 0;
					if(tempInternal.NW != null){
						if(tempInternal.NW.getClass().equals(Leaf.class))
							checkQuad[2] = 1;
						else 
							checkQuad[2] = 0;
					}
					else
						checkQuad[2] = 0;
					check = checkQuad[0] + checkQuad[1] + checkQuad[2];
					if(check > 1 || check == 0)
						tempInternal.SE = null;
					else if(checkQuad[0] == 1){
						Leaf temp = (Leaf) tempInternal.SW;
						temp.value = tempInternal.value;
						return h = temp;
					}
					else if(checkQuad[1] == 1){
						Leaf temp = (Leaf) tempInternal.NE;
						temp.value = tempInternal.value;
						return h = temp;
					}
					else if(checkQuad[2] == 1){
						Leaf temp = (Leaf) tempInternal.NW;
						temp.value = tempInternal.value;
						return h = temp;
					}
				}
				h = tempInternal;
			}
		}
		return h;
	}
	int getDirection(Key x, Key y, int[] value){
		int direction = -1;
			System.out.println("gettingDirection");
			int midX = value[2]/2;
			int midY = value[3]/2;
			//add node in approiet partition
			if(less(x,(Key)(Integer)midX) && less(y,(Key)(Integer)midY)){
				return 0;
			}
			else if(less(x,(Key)(Integer)midX) && !less(y,(Key)(Integer)midY)){
				return 1;
			}
			else if(!less(x,(Key)(Integer)midX) && less(y,(Key)(Integer)midY)){
				return 2;
			}
			else if(!less(x,(Key)(Integer)midX) && !less(y,(Key)(Integer)midY)){
				return 3;
			}
		return direction;
	}
    private boolean less(Key k1, Key k2) { return k1.compareTo(k2) <  0; }
    private boolean eq  (Key k1, Key k2) { return k1.compareTo(k2) == 0; }
}
class PartitionRect{
	int xMin,xMax,yMin,yMax;
	int[] value = new int[4];
	public PartitionRect(){
	}
	void setValue(int xMin, int yMin, int xMax, int yMax){
		this.xMin = xMin;
		this.xMax = xMax;
		this.yMin = yMin;
		this.yMax = yMax;
		value[0] = xMin;
		value[1] = yMin;
		value[2] = xMax;
		value[3] = yMax;
	}
	void interval(int direction){
		if(xMin != 0){
			xMax = Math.abs(xMin-xMax);
			xMin = 0;
			//System.out.println("IFX "+xMin+","+xMax);
		}
		if(yMin != 0){
			yMax = Math.abs(yMin-yMax);
			yMin = 0;
			//System.out.println("IFY "+yMin+","+yMax);
		}
		switch(direction){
		case 0://NW(if we are going in NW direction only xmax,ymax need to change)
			xMax /= 2;
			yMax /= 2;
			value[2] -= xMax;
			value[3] -= yMax;
			break;
		case 1://SW(if we are going in SW direction only xmax,ymin need to change)
			xMax /= 2;
			yMin = (yMax/2);
			value[2] -= xMax;
			value[1] += yMin;
			break;
		case 2://NE(if we are going in NE direction only xmin,ymax need to change)
			xMin = (xMax/2);
			yMax /= 2;
			value[0] += xMin;
			value[3] -= yMax;
			break;
		case 3://SE(if we are going in SE direction only xmin,ymin need to change)
			xMin = (xMax/2);
			yMin = (yMax/2);
			value[0] += xMin;
			value[1] += yMin;
			break;
		}
	}
	int[] getValue(){
		return value;
	}
}//endclass PartitionRect
class FindQuadrantMid{
	int x, y;
	int xMin,yMin,xMax,yMax;
	FindQuadrantMid(){
	}
	void setValue(int xMin, int yMin, int xMax, int yMax){
		this.xMin = xMin;
		this.yMin = yMin;
		this.xMax = xMax;
		this.yMax = yMax;
		this.x = xMin;
		this.y = yMin;
		calculate();
	}
	void calculate(){
		if(xMin != 0){
			xMax = Math.abs(xMin-xMax);
			xMin = 0;
			//System.out.println("IFX "+xMin+","+xMax);
		}
		if(yMin != 0){
			yMax = Math.abs(yMin-yMax);
			yMin = 0;
			//System.out.println("IFY "+yMin+","+yMax);
		}
		x += (xMax/2);
		y += (yMax/2);
	}
	int getMidX(){
		return x;
	}
	int getMidY(){
		return y;
	}
}//endclass FindQuadrantMid
