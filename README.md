# mdl-et718-20220427-rails-hr-repo


rails generate model Article title:string text:text
rails generate scaffold Resume name image_url role locatin email phone


rails generate model Comment commenter:string body:text article:references
rails generate model Skill title level resume:references

has_many :comments, dependent: :destroy
has_many :skills, dependent: :destroy

resources :articles do
  resources :comments
end
resources :resumes do
  resources :skills
end


rails generate controller Comments
rails generate controller Skills

class CommentsController < ApplicationController
  def create
    @article = Article.find(params[:article_id])
    @comment = @article.comments.create(comment_params)
    redirect_to article_path(@article)
  end
 
  private
    def comment_params
      params.require(:comment).permit(:commenter, :body)
    end
end

class SkillsController < ApplicationController
  def create
    @resume = Resume.find(params[:resume_id])
    @skill = @resume.skills.create(skill_params)
    redirect_to resume_path(@resume)
  end
 
  private
    def skill_params
      params.require(:skill).permit(:title, :level)
    end
end


<p>
  <strong>Commenter:</strong>
  <%= comment.commenter %>
</p>
 
<p>
  <strong>Comment:</strong>
  <%= comment.body %>
</p>

<p>
  <strong>Skill Title</strong>
  <%= skill.title %>
</p>
 
<p>
  <strong>Level:</strong>
  <%= skill.level %>
</p>