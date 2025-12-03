import csv

def load_data(char_csv_path, word_csv_path):
    char_scores = {}
    try:
        with open(char_csv_path, 'r', encoding='utf-8') as f:
            reader = csv.reader(f)
            for row in reader:
                if len(row) < 2: continue
                try:
                    char_scores[row[0].strip()] = int(row[1])
                except ValueError: continue
    except FileNotFoundError:
        print(f"ファイルが見つかりません: {char_csv_path}")
        return None, None

    valid_words = []
    try:
        with open(word_csv_path, 'r', encoding='utf-8') as f:
            reader = csv.reader(f)
            for row in reader:
                if row: valid_words.append(row[0].strip())
    except FileNotFoundError:
        print(f"ファイルが見つかりません: {word_csv_path}")
        return None, None

    return char_scores, valid_words

def solve_unique_score(char_csv_path, word_csv_path):
    char_scores, valid_words = load_data(char_csv_path, word_csv_path)
    if not char_scores: return

    excluded_chars = set()
    position_constraints = {}

    print("\n=== 五文字単語 ソルバー（重複加算なし版） ===")
    print("【コマンド】")
    print(" 1. 文字のみ (例: 'ヌ')   -> 除外リストに追加")
    print(" 2. 数字:文字 (例: '2:ン') -> 2文字目を'ン'に固定")
    print(" 3. 'reset' -> リセット, 'exit' -> 終了")

    while True:
        results = []

        for word in valid_words:
            # 1. 位置固定チェック
            match_position = True
            for idx, char in position_constraints.items():
                if idx >= len(word) or word[idx] != char:
                    match_position = False
                    break
            if not match_position:
                continue

            # 2. 除外文字チェック
            if any(char in excluded_chars for char in word):
                continue

            # 3. スコア計算
            # 単語内の重複を取り除いたセットを作る
            unique_chars = set(word)
            
            current_score = 0
            possible = True
            
            # 重復を取り除いた文字たちでチェックと計算を行う
            for char in unique_chars:
                if char not in char_scores:
                    possible = False
                    break
                current_score += char_scores[char]
            
            if possible:
                if not (word, current_score) in results:
                    results.append((word, current_score))

        # --- ソートと表示 ---
        results.sort(key=lambda x: (-x[1], x[0]))
        
        pattern_display = ["＊"] * 5
        for i in range(5):
            if i in position_constraints:
                pattern_display[i] = position_constraints[i]
        
        print("\n" + "="*50)
        print(f"現在のパターン: {''.join(pattern_display)}")
        print(f"除外中の文字: {sorted(list(excluded_chars))}")
        print(f"候補単語数: {len(results)}")
        print("-" * 50)
        
        if results:
            print("【スコア上位】")
            for word, score in results[:10]:
                print(f"  {word} : {score}点")
        else:
            print("  条件を満たす単語はありません。")
        print("="*50)

        # --- ユーザー入力 ---
        user_input = input("\n>>> コマンド: ").strip()

        if user_input.lower() == 'exit': break
        elif user_input.lower() == 'reset':
            excluded_chars.clear()
            position_constraints.clear()
            print("リセットしました。")
        elif ':' in user_input:
            try:
                pos_str, char_part = user_input.split(':', 1)
                pos_idx = int(pos_str) - 1
                if 0 <= pos_idx <= 4:
                    position_constraints[pos_idx] = char_part.strip()
            except ValueError: pass
        else:
            for c in user_input:
                excluded_chars.add(c)

solve_unique_score('char.csv', 'answer.csv')
