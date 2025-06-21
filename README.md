class SearchSuggestionSystem {
    /**
     * @param {string[]} products
     */
    constructor(products) {
        // Sort products initially for lexicographical order.
        // This helps maintain sorted suggestions later and optimizes filtering.
        this.products = products.sort();
    }

    /**
     * @param {string} searchWord
     * @return {string[][]}
     */
    getSuggestions(searchWord) {
        const result = [];
        let currentPrefix = "";

        for (let i = 0; i < searchWord.length; i++) {
            currentPrefix += searchWord[i];
            const suggestionsForPrefix = [];

            // Filter products that start with the currentPrefix
            for (const product of this.products) {
                if (product.startsWith(currentPrefix)) {
                    suggestionsForPrefix.push(product);
                }
                // Optimization: Since products are sorted, if a product no longer
                // starts with the prefix, subsequent products won't either (unless
                // the prefix is very short and the products are very different,
                // but for effective prefix matching, this is generally true).
                // However, a simple startsWith check is often sufficient and clear.
            }

            // Take up to 3 suggestions
            result.push(suggestionsForPrefix.slice(0, 3));
        }

        return result;
    }
}

// Example Usage:
const products = ["mobile", "mouse", "moneypot", "monitor", "mousepad"];
const searchWord = "mouse";

const system = new SearchSuggestionSystem(products);
const suggestions = system.getSuggestions(searchWord);
console.log(suggestions);
/*
Expected Output:
[
  ["mobile", "moneypot", "monitor"],
  ["mobile", "moneypot", "monitor"],
  ["mouse", "mousepad"],
  ["mouse", "mousepad"],
  ["mouse", "mousepad"]
]
*/

const products2 = ["bags", "baggage", "banner", "bike", "bottle"];
const searchWord2 = "bags";
const system2 = new SearchSuggestionSystem(products2);
const suggestions2 = system2.getSuggestions(searchWord2);
console.log(suggestions2);
/*
Expected Output:
[
  ["bags", "baggage", "banner"],
  ["bags", "baggage", "banner"],
  ["bags", "baggage"]
]
*/

const products3 = ["apple", "apricot", "banana"];
const searchWord3 = "ap";
const system3 = new SearchSuggestionSystem(products3);
const suggestions3 = system3.getSuggestions(searchWord3);
console.log(suggestions3);
/*
Expected Output:
[
  ["apple", "apricot"],
  ["apple", "apricot"]
]
*/
