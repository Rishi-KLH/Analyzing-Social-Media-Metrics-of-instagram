# Analyzing-Social-Media-Metrics-of-instagram
import pandas as pd
import matplotlib.pyplot as plt

class SocialMediaAnalytics:
    def init(self, engagement_data, demographic_data):
      
        self.engagement_data = pd.read_csv(engagement_data)  # Engagement data file
        self.demographic_data = pd.read_csv(demographic_data)  # Demographic data file

    def calculate_engagement_rate(self):
        
        self.engagement_data['Engagement Rate (%)'] = (
            (self.engagement_data['Likes'] + self.engagement_data['Comments'] + self.engagement_data['Shares']) /
            self.engagement_data['Reach'] * 100
        )
        avg_engagement_rate = self.engagement_data['Engagement Rate (%)'].mean()  
        print(f"Average Engagement Rate: {avg_engagement_rate:.2f}%")
        return avg_engagement_rate

    def calculate_follower_growth(self):
        
        initial_followers = self.engagement_data['Followers'].iloc[0]
        final_followers = self.engagement_data['Followers'].iloc[-1]
        growth = ((final_followers - initial_followers) / initial_followers) * 100 
        print(f"Follower Growth: {growth:.2f}%")
        return growth

    def identify_top_posts(self, top_n=3):
        # Identify the top N posts with the highest engagement
        self.engagement_data['Total Engagement'] = (
            self.engagement_data['Likes'] + self.engagement_data['Comments'] + self.engagement_data['Shares']
        )
        top_posts = self.engagement_data.nlargest(top_n, 'Total Engagement')  # Get top N posts
        print(f"Top {top_n} Performing Posts:")
        print(top_posts[['Post', 'Total Engagement']])
        return top_posts

    def demographic_breakdown(self):
        # Summarize demographics based on the data
        demographics = self.demographic_data.groupby('Category').sum()  # Group by category and sum followers
        print("Demographic Breakdown:")
        print(demographics)
        return demographics

    def generate_report(self):
      
        engagement_rate = self.calculate_engagement_rate()
        growth = self.calculate_follower_growth()
        top_posts = self.identify_top_posts()
        demographics = self.demographic_breakdown()

      
        plt.figure(figsize=(10, 6))
        plt.plot(self.engagement_data['Date'], self.engagement_data['Engagement Rate (%)'], label='Engagement Rate')
        plt.title("Engagement Rate Over Time (Instagram)")
        plt.xlabel("Date")
        plt.ylabel("Engagement Rate (%)")
        plt.xticks(rotation=45)
        plt.legend()
        plt.tight_layout()
        plt.show()

        
        demographics.plot(kind='bar', figsize=(8, 5), title="Demographic Breakdown (Instagram)")
        plt.tight_layout()
        plt.show()

        return {
            "Engagement Rate": engagement_rate,
            "Follower Growth": growth,
            "Top Posts": top_posts,
            "Demographics": demographics
        }

# Usage Example
# Replace 'engagement.csv' and 'demographics.csv' with actual Instagram analytics data files
analytics = SocialMediaAnalytics('/content/engagement (1).csv', '/content/demographics (1).csv')
report = analytics.generate_report()

# Print the report for verification
print(report)
