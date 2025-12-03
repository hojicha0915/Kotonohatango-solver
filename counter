import csv
from collections import Counter

word_list = []
try:
    with open('answer.csv', mode='r', encoding='utf-8-sig') as f:
        reader = csv.reader(f)
        for row in reader:
            if row: 
                word_list.append(row[0])
except FileNotFoundError:
    print("エラー: 'answer.csv' が見つかりませんでした。")
    exit()

final_counts = Counter()

for word in word_list:
    unique_chars_in_word = set(word)
    final_counts.update(unique_chars_in_word)

print("\n文字の出現回数（1単語につき1回までカウント）:")
print(final_counts)

print("\n多い順:")
print(final_counts.most_common())
