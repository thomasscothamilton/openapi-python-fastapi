'''python
def wordPatternMatch(pattern: str, s: str) -> bool:
    def backtrack(p_index, s_index, mapping, used):
        # Base case: both pattern and string are fully matched
        if p_index == len(pattern) and s_index == len(s):
            return True
        # Mismatch in lengths (only one exhausted)
        if p_index == len(pattern) or s_index == len(s):
            return False

        current_char = pattern[p_index]

        # Try all possible substring candidates starting from s_index
        for end_index in range(s_index + 1, len(s) + 1):
            candidate = s[s_index:end_index]

            # Case 1: current_char is already mapped
            if current_char in mapping:
                # It must match the current candidate to continue
                if mapping[current_char] != candidate:
                    continue
                # Recurse with the next pattern character and matched substring
                if backtrack(p_index + 1, end_index, mapping, used):
                    return True

            # Case 2: current_char is not mapped yet
            else:
                # Ensure this candidate hasn't been mapped to another character
                if candidate in used:
                    continue

                # Try this candidate as a new mapping
                mapping[current_char] = candidate
                used.add(candidate)

                if backtrack(p_index + 1, end_index, mapping, used):
                    return True

                # Backtrack: undo changes
                del mapping[current_char]
                used.remove(candidate)

        # No valid mapping found for this configuration
        return False

    # Initial recursive call
    return backtrack(0, 0, {}, set())
'''