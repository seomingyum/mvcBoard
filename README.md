### MVC게시판(Spring Framwork, MyBatis, tiles)

#### 1.SQL
  1-1.board.xml
  
  	<select id="selectAllBoardList" parameterType="Map" resultMap="boardResult">
     	select * 
     	from(
     	select rownum rn, x.*
     	from(
         select * from t_board 
         <where>
	         articleType = #{articleType}
	         <if test='title != null and title!=""'>
	         and title like '%'||#{title}||'%'
	         </if>
	         <if test="content != null and content!=''">
	         and content like '%'||#{content}||'%'
	         </if>
	         <if test="id != null and id != ''">
	         and id like '%'||#{id}||'%'
	         </if>
        </where>  
          order by writeDate desc
          ) x
          )
          <where>
          	rn <![CDATA[>]]> #{param1} and rn <![CDATA[<=]]> #{param2}
          </where>	 	
	</select>

	<select id="selectArticle" parameterType="int" resultType="boardVo">
         select * from t_board
         <where>
	         articleNO = #{articleNO}
        </where>  	 	
	</select> 

 	 <delete id="deleteArticle"  parameterType="int">
		   delete from t_board
		   where
		   articleNO = #{articleNO}
	 </delete>

  <insert id="insertArticle"  parameterType="boardVo">
		 insert into t_board(articleNO, title, 
		 <if test="content!=null">
		 content, 
		 </if>
		 id, 
		 <if test='fileName_1!=null'>
		 fileName_1,
		 </if>
		 <if test='fileName_2!=null'> 
		 fileName_2,
		 </if> 
		 articleType)
		 values(no_seq.nextval, #{title}, 
		 <if test="content!=null">
		 #{content, jdbcType=VARCHAR}, 
		 </if>
		 #{id},
		 <if test='fileName_1!=null'>
		 #{fileName_1},
		 </if>
		 <if test='fileName_2!=null'> 
		 #{fileName_2},
		 </if> 
		 #{articleType}) 
	</insert>

  <update id="updateArticle"  parameterType="boardVo">
     update t_board
     <set>
	     title=#{title}, content=#{content},
	     <if test="fileName_1!=null">
	     	fileName_1=#{fileName_1}, 
	     </if>
	     <if test="fileName_2!=null">
	     	fileName_2=#{fileName_2},
	     </if>
	 </set>   
     where
     articleNO=#{articleNO}
   </update>

   1-2.memebr.xml
   	<select id="getLoginMember"  resultType="memberVo"   parameterType="memberVo" >
  		select * from t_member	
  		where id=#{id}		
  	</select>

    <select id="checkIsMember" parameterType="memberVo" resultType="memberVo">
		 	select * from t_member	
			where id=#{id}      
	  </select>

  	<insert id="insertMember"  parameterType="memberVo">
  		insert into t_member(id,pwd, name, email)
  		values(#{id}, #{pwd}, #{name}, #{email})      
	  </insert>   
