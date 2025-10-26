# project- 
import json
import matplotlib.pyplot as plt
import os
from typing import List, Dict, Any


CHART_OUTPUT_FILENAME = "sales_chart.png"
SALES_DATA = [
{"month": "Jan", "sales": 120000},
{"month": "Feb", "sales": 155000},
{"month": "Mar", "sales": 130000},
{"month": "Apr", "sales": 185000},
{"month": "May", "sales": 210000},
{"month": "Jun", "sales": 195000}
]


def format_currency(value: float) -> str:
return f"${value:,.0f}"


def generate_chart(data, filename):
months = [d['month'] for d in data]
sales = [d['sales'] for d in data]
max_sales, min_sales = max(sales), min(sales)
colors = ['#10b981' if s == max_sales else ('#ef4444' if s == min_sales else '#3b82f6') for s in sales]


plt.style.use('dark_background')
fig, ax = plt.subplots(figsize=(10, 6))
ax.bar(months, sales, color=colors, edgecolor='white', linewidth=0.5)
ax.set_title('Sales Trend Visualization', fontsize=16, color='#e5e7eb')
ax.set_xlabel('Month', fontsize=12, color='#9ca3af')
ax.set_ylabel('Total Sales ($)', fontsize=12, color='#9ca3af')
formatter = plt.FuncFormatter(lambda x, pos: format_currency(x))
ax.yaxis.set_major_formatter(formatter)
plt.tight_layout()
plt.savefig(filename, dpi=300)
plt.close(fig)
return filename


def generate_nlg_report(data):
sales_values = [d['sales'] for d in data]
total_sales, avg_sales = sum(sales_values), sum(sales_values) / len(sales_values)
max_sale, min_sale = max(sales_values), min(sales_values)
max_month = next(d['month'] for d in data if d['sales'] == max_sale)
min_month = next(d['month'] for d in data if d['sales'] == min_sale)
growth_rate = (sales_values[-1] - sales_values[0]) / sales_values[0] * 100


if growth_rate > 5:
trend = "a strong positive upward trend."
elif growth_rate < -5:
trend = "a significant negative decline."
else:
trend = "a generally stable trend."


return f"Total Revenue: {format_currency(total_sales)} | Average: {format_currency(avg_sales)}\nHighest Month: {max_month} ({format_currency(max_sale)}) | Lowest: {min_month} ({format_currency(min_sale)})\nOverall Trend: {trend}"


def main():
chart = generate_chart(SALES_DATA, CHART_OUTPUT_FILENAME)
report = generate_nlg_report(SALES_DATA)
print("\n=== Final Sales Report ===\n")
print(report)
print(f"\nChart saved as: {os.path.abspath(chart)}")


if __name__ == "__main__":
main()
