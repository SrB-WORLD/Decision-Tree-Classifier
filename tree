#include <iostream>
#include <vector>
#include <string>
#include <cstdlib>
#include <unordered_map>
#include <map>
#include <cmath>


struct TreeNode;
struct EdgeNode;
 
typedef std::string tree_t;
 
struct EdgeNode{
    tree_t val;
    TreeNode* subtree;
    EdgeNode* next;
};
 
struct TreeNode{
    tree_t val;
    EdgeNode* subtree_l;
};

TreeNode* allocate_tree_node(tree_t e){
  TreeNode* tmp = new TreeNode;
  tmp->val = e;
  tmp->subtree_l = NULL;
  return tmp;
}

EdgeNode* cons_edge_node(TreeNode* t, EdgeNode* subtree_l, tree_t e) {
  EdgeNode* tmp = new EdgeNode;
  tmp->val = e;
  tmp->subtree = t;
  tmp->next = subtree_l;
  return tmp;
}

//gets the column based on the index and returns it as a 1D vector
std::vector<std::string> get_column(const std::vector<std::vector<std::string>>& data, int column_index) {
    std::vector<std::string> column;
    for (int i = 1; i < data.size(); i++) {

        column.push_back(data[i][column_index]);
    }
    return column;
}

//return a 2D vector after we get the order of vector<int>
std::vector<std::vector<std::string>> reorderColumns(const std::vector<std::vector<std::string>>& value_in, const std::vector<int>& order) {

    std::vector<std::vector<std::string>> new_table;

    for (size_t rowIndex = 0; rowIndex < value_in.size(); ++rowIndex) {

        std::vector<std::string> row = value_in[rowIndex];
        std::vector<std::string> newRow;
        for (size_t i = 0; i < order.size(); ++i) {

            newRow.push_back(row[order[i]]);
            
        }   

        new_table.push_back(newRow);
    
    }

    return new_table;


}
//uses entropy equation on a column and returns it
double calculate_entropy(const std::vector<std::string>& column) {
    std::unordered_map<std::string, int> subset_counter;

    for (auto index = 0; index < column.size(); index++) {

        const std::string& value = column[index];
   
        if (subset_counter.find(value) == subset_counter.end()) {
    
            subset_counter[value] = 1;
        } 
        else {
        
            subset_counter[value]++;
        }
      
    }

    double entropy = 0.0;
    double total = column.size();
    for (auto subset = subset_counter.begin(); subset != subset_counter.end(); ++subset) {

        double p = subset->second / total;
        entropy -= p * log2(p);
    }

    return entropy;
}



//Uses string to get the ccorrespondent values from the last column
double get_subset_entropy(const std::vector<std::vector<std::string>>& data, std::string value, int column){
    std::vector<std::string> req_attribute_col = get_column(data,column);
    std::vector<std::string> attributes = data[0];

    std::vector <std::string> last_col = get_column(data, attributes.size()-1);

    std::vector<std::string> subset_col;
    for (int i = 0; i < req_attribute_col.size(); i++){
        if (req_attribute_col[i] == value){
            subset_col.push_back(last_col[i]);
        }
    }
    return calculate_entropy(subset_col);
}


//calculates information gain for a specific attribute so we know which node should be first
double calculate_information_gain(const std::vector<std::vector<std::string>>& data, double last_entropy, int index) {
    std::unordered_map<std::string, int> subsets; 
    std::vector<std::string> column = get_column(data,index);


    
    for (auto index = 0; index < column.size(); index++) {

        const std::string& value = column[index];
   
        if (subsets.find(value) == subsets.end()) {
    
            subsets[value] = 1;
        } 
        else {
    
            subsets[value]++;
        }
    }

    double average_entropy = 0.0;
    for (auto it = subsets.begin(); it != subsets.end(); it++) {
        
        double weight = it->second / (double)column.size();

        double subset_entropy = get_subset_entropy(data, it->first,index);
        
        average_entropy += weight * subset_entropy;

    }

    return last_entropy - average_entropy;
}

class A3Tree{
 
public: 

    A3Tree(const std::vector<std::vector<std::string>>& value_in){
        std::vector<std::string> attributes = value_in[0];

        std::vector<std::string> last_column = get_column(value_in, attributes.size()-1);
        double last_col_entropy = calculate_entropy(last_column);

        std::vector<double> info_gain;
        double tmp;
        for (size_t i = 0; i < attributes.size()-1; i++){
            tmp = calculate_information_gain(value_in, last_col_entropy,i);
            info_gain.push_back(tmp);

        }

        
    
        std::vector<double> ordered_gain = info_gain;

        for (size_t i = 0; i < ordered_gain.size(); i++) {  
            for (size_t j = 0; j < ordered_gain.size() - 1 - i; j++) { 
                if (ordered_gain[j] < ordered_gain[j + 1]) { 
                    std::swap(ordered_gain[j], ordered_gain[j + 1]);
                }       
            }
        }
        
        for(int i = 0 ; i < info_gain.size(); i++){
            for(int j = 0 ; j < info_gain.size(); j++){
                if(ordered_gain[i] == info_gain[j]){
                    attribute_index_order.push_back(j);
                }
            }
        }
        
        std::vector<int> column_order_index = attribute_index_order;
        column_order_index.push_back(attributes.size()-1);



        
        
        std::vector<std::vector<std::string>> ordered_columns = reorderColumns(value_in,column_order_index);
    

        t = allocate_tree_node(ordered_columns[0][0]);

        

        for (size_t i = 1; i < ordered_columns.size(); i++) {
            insert_data(t,ordered_columns[0], ordered_columns[i],1);
        }
        
        
        destroy_irrelevant_nodes(t,NULL);



    }
                                                                                                         
    void insert_data(TreeNode* node, const std::vector<std::string>& attributes, const std::vector<std::string>& data, int index) {
        
        EdgeNode* current = node->subtree_l;
        EdgeNode* prev = NULL;

        while (current != NULL && current->val != data[index - 1]) {
            prev = current;
            current = current->next;
        }

        if(current == NULL){
            TreeNode* new_subtree = NULL;
            if (index < data.size() - 1) {
                new_subtree = allocate_tree_node(attributes[index]);
            }
            
            EdgeNode* new_edge = cons_edge_node(new_subtree,NULL,data[index - 1]);
            if (prev == NULL) {
                node->subtree_l = new_edge;
            } 
            else {
                prev->next = new_edge;
            }
            current = new_edge;
        }   
        

        if (index == data.size() - 1) {

            TreeNode* leafNode = allocate_tree_node(data.back());
            current->subtree = leafNode;
        } 
        else {
          
            insert_data(current->subtree, attributes, data, index + 1);
        }


    }

    //if sibling edgeNode parent of a leafNode has the same values we return true to further prune the tree
    bool sibling_same_leafNode(EdgeNode* first,std::string leafValue ){

        for (EdgeNode* node = first; node != NULL; node = node->next) {
            if (node->subtree->subtree_l != NULL || node->subtree->val != leafValue) {
                return false; 
            }
        }
        return true; 


    }

    //destroys node with edgeNode that doesn't have sibling or siblings with has same value for leafNode to further prune the tree    
    void destroy_irrelevant_nodes(TreeNode* t,EdgeNode* parentEdge){
    EdgeNode* current = t->subtree_l;
    EdgeNode* prev = NULL;
    EdgeNode* next;

    while (current != NULL) {
        next = current->next;

       
        if (current->subtree->subtree_l != NULL) {
            
            destroy_irrelevant_nodes(current->subtree, current);
    
        }

      
            
        if (current->subtree->subtree_l == NULL){
            std::string leafValue = current->subtree->val;

            TreeNode* leafNode = current->subtree;

            if ((prev == NULL && next == NULL) || (sibling_same_leafNode(t->subtree_l, leafValue))) {
                if (parentEdge != NULL) {
                    parentEdge->subtree = leafNode;

                    EdgeNode* tmp = current;
                    while (tmp != NULL) {
                        next = tmp->next;
                        delete tmp;  // Delete the current node
                        tmp = next;
                    }

                    delete t;  
                    return; 
                } 
    
            }
            
        }
   
    
        prev = current;
        current = next;
    }
     
    }


    //order query based on ordered nodes
    std::string query(const std::vector<std::string>& q) {
        std::vector<std::string> ordered_query;
        for (int i = 0; i < q.size();i++){
            ordered_query.push_back(q[attribute_index_order[i]]);
        }
        return search(t, ordered_query, 0);
        
    }   
    
    //searching through the tree
    std::string search(TreeNode* tnode,const std::vector<std::string>& q, int index){
        EdgeNode* edgenode_current = tnode->subtree_l;

        while (edgenode_current != NULL ){
            if (edgenode_current->val == q[index]){
                if(edgenode_current->subtree->subtree_l ==NULL ){
                    return edgenode_current->subtree->val;
                }
                else{
                    return search(edgenode_current->subtree, q,index +1);
                }


            }
            edgenode_current = edgenode_current->next;
            
        }
        return "Attribute not found";
    }
    

    int node_count() {
        return count_nodes(t);
    }
    
    int count_nodes(TreeNode* node) const {
       
        int count = 1; // Count the current node
        
        EdgeNode* edge = node->subtree_l;
        while (edge != NULL) {
            count += count_nodes(edge->subtree);
            edge = edge->next;
        } // Recursively count the nodes in the subtrees
        
        return count;
    }

    int leaf_node_count() {
       return count_leaf_nodes(t);
    }

     int count_leaf_nodes(TreeNode* node) {
   
        if (node->subtree_l == NULL) {
            return 1; // if the node has no children, it is a leaf node
        }

        int count = 0;
        EdgeNode* edge = node->subtree_l;
        while (edge != NULL) {
            count += count_leaf_nodes(edge->subtree);
            edge = edge->next;
        }

        return count;
    }

    ~A3Tree(){
        deallocate_tree(t);
    }

    void deallocate_tree(TreeNode* node){
        
        EdgeNode* current_edge = node->subtree_l;

        while(current_edge!= NULL){
            deallocate_tree(current_edge->subtree);
            EdgeNode* next = current_edge->next;
            delete current_edge;
            current_edge = next;
        }

    
        delete node;

    }

   
 
private:
 
    TreeNode* t;
    std::vector<int> attribute_index_order;
    
};

int main(){ 
 
    // direct initialisation of a vector
    // in this case it's a vector containing vectors
    // each of which contains words (std::string)
    std::vector<std::vector<std::string>> input
    {
        {"temperature", "rain", "wind", "quality"},
        {"high", "yes", "light", "acceptable"},
        {"low", "yes", "light", "acceptable"},
        {"low", "no", "moderate", "good"},
        {"high", "yes", "strong", "poor"},
        {"high", "yes", "moderate", "acceptable"},
        {"high", "no", "moderate", "good"},
        {"low", "yes", "strong", "poor"},
        {"high", "no", "light", "good"},
        {"low", "yes", "moderate", "poor"},
        {"high", "no", "strong", "poor"}
    };
 
    A3Tree t(input);

    
 
    // direct initialisation of a vector:
    std::vector<std::string> q{"low", "no", "moderate"};
 
    std::cout << t.query(q) << std::endl;
    // this should print: good
 
    // assigning new content overwriting the previous content:
    q =  {"high", "yes", "light"};
 
    std::cout << t.query(q) << std::endl;
    // this should print: acceptable

    std::cout << t.node_count() << std::endl;
    // this depends on the actual tree generated,
    // if we consider the tree in the example which
    // has wind in the root node this should print: 10

    
    std::cout << t.leaf_node_count() << std::endl;
    // this depends on the actual tree generated,
    // if we consider the tree in the example which
    // has wind in the root node this should print: 6
 

    std::vector<std::vector<std::string>> input2
    {
    {"Feature_3", "feature2", "feature", "feature0", "not_a_feature"},
    {"a13480", "10", "a13480", "a", "1"},
    {"B_34203", "9", "1343432", "a", "a2"},
    {"a13480", "8", "57657", "a", "3"},
    {"B_34203", "B_34203", "4523", "a", "2"},
    {"B_34203", "6", "4523", "a", "some_value"},
    {"a13480", "5", "4523", "a", "1"}
    };

    A3Tree t2(input2);
    q = {"B_34203", "9", "1343432", "a"};
    std::cout << t2.query(q) << std::endl;
    // this should print: a2
    
    std::cout<<t2.node_count()<<std::endl;

    std::cout << t2.leaf_node_count()<<std::endl;
 
  

}
