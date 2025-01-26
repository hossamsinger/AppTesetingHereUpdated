        /// <summary>
        /// Returns data load options required for DevExtreme datagrid
        /// </summary>
        /// <param name="loadOptions"></param>
        /// <returns></returns>
        [HttpGet("dxdatagrid")]
        public async Task<IActionResult> DxDataGrid(DataSourceLoadOptions loadOptions)  // Use ActionResult instead of HttpResponseMessage
        {
            var questions = await _context.Questions
                .AsNoTracking()
                .Include(e => e.Answers)
                .Include(e => e.QuestionType)
                .OrderBy(e => e.Id)
                .Select(e => new
                {
                    e.Id,
                    e.Name,
                    e.Marks,
                    e.IsPublic,
                    e.QuestionLevel,
                    QuestionTypeName = e.QuestionType.Name,
                    QuestionTypeId = e.QuestionType.Id,
                    e.Answers
                })
                .ToListAsync();

            var result = DataSourceLoader.Load(questions, loadOptions);
            return Ok(result);  // Return Ok response with the result
        }

