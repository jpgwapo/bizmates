### Simple UI and UX for weather forecast for Bizmates Philippines, Inc. 

1.	Write a query to display the ff columns ID (should start with T + 11 digits of trn_teacher.id with leading zeros like 'T00000088424'), Nickname, Status and Roles (like Trainer/Assessor/Staff) using table trn_teacher and trn_teacher_role.

				SELECT concat("T00000", trn_teacher_role.teacher_id) as ID, nickname as Nickname, 
				CASE
					WHEN status = 1 THEN 'open'
					WHEN role = 2 THEN 'backup'
					ELSE 'reserved'
				END AS Status,
				CASE
					WHEN role = 1 THEN 'trainer'
					WHEN role = 2 THEN 'assessor'
					ELSE 'staff'
				END AS Role
				FROM bizmates.trn_teacher
				join trn_teacher_role on  trn_teacher.id = trn_teacher_role.teacher_id;

2.	Write a query to display the ff columns ID (from teacher.id), Nickname, Open (total open slots from trn_teacher_time_table), Reserved (total reserved slots from trn_teacher_time_table), Taught (total taught from trn_evaluation) and NoShow (total no_show from trn_evaluation) using all tables above. Should show only those who are active (trn_teacher.status = 1 or 2) and those who have both Trainer and Assessor role.

			select trn_teacher.id as ID, nickname, 
			count(case when trn_time_table.status = 1 then 1 end) as Open,
			count(case when trn_time_table.status = 3 then 1 end) as Reserved,
			count(case when trn_evaluation.result = 1 then 1 end) as Taught,
			count(case when trn_evaluation.result = 2 then 1 end) as NoShow
			from trn_time_table 
			join trn_evaluation on trn_time_table.teacher_id = trn_evaluation.teacher_id
			join trn_teacher on trn_teacher.id = trn_time_table.teacher_id
			join trn_teacher_role on trn_teacher_role.teacher_id = trn_teacher.id
			where (trn_teacher.status = 1 or 2)
			and (trn_teacher_role.role = 1 AND 2)
			group by nickname, id;

