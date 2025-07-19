# q-no-1233
import java.util.*;

class Solution {
    class TrieNode {
        boolean wordEnd = false;
        Map<String, TrieNode> child = new HashMap<>();
    }
    private void trieInsert(String folder, TrieNode curr) {
        int i = 1;
        while (i < folder.length()) {
            StringBuilder name = new StringBuilder();
            while (i < folder.length() && folder.charAt(i) != '/') { 
                name.append(folder.charAt(i));
                i++;
            }

            if (curr.wordEnd) return; 
            String nameStr = name.toString();
            if (!curr.child.containsKey(nameStr)) 
                curr.child.put(nameStr, new TrieNode());
            
            curr = curr.child.get(nameStr);
            i++;
        }
        curr.wordEnd = true;
    }


    private void searchTrie(TrieNode curr, List<String> res, String path) {
        if (curr == null) return;
        if (curr.wordEnd) {
            res.add(path.substring(0, path.length() - 1)); // Remove trailing '/'
            return;
        }

        for (Map.Entry<String, TrieNode> entry : curr.child.entrySet()) {
            searchTrie(entry.getValue(), res, path + entry.getKey() + "/");
        }
    }

    public List<String> removeSubfolders(String[] folders) {
        TrieNode root = new TrieNode();

        // Insert all folders into the Trie
        for (String folder : folders)
            trieInsert(folder, root);
        
        List<String> res = new ArrayList<>(); // Stores result
        searchTrie(root, res, "/");
        return res;
    }
}
