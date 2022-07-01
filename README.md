zhangfuxing

# java 学习
## java se
![](https://cdn.colorhub.me/3ZLafHzpulg/rs:auto:0:500:0/g:sm/fn:colorhub/bG9jYWw6Ly8vN2MvYWMvMmVhNmU1YWE5YmZlYjE3NzNmY2E0YWU0YjVlZjM5NjFkYTkzN2NhYy5qcGVn.jpg)

# 数据库学习
## MySQL
### 1.初识数据库

### 2.数据库基础

# 框架学习

--------------------------------------
 private Integer projectId;
    private String projectName;
    /**
     * 项目数
     */
    private Integer totalProject;
    /**
     * 项目类型，2D或3D
     */
    private String projectType;
    /**
     * 年度
     */
    private Integer yearNo;
    /**
     * 工区
     */
    private String workAreaName;
    /**
     * 测/束线
     */
    private Integer swathNum;
    /**
     * sps文件
     */
    private Integer spsNum;
    /**
     * 测线长度/施工面积
     */
    private String lineLength;
    /**
     * 覆盖长度/覆盖面积
     */
    private String lineFullLength;
    /**
     * 一次覆盖面积
     */
    private String oneFoldArea;
    /**
     *炮点长度/面积
     */
    private String shotArea;
    /**
     * 检点波
     */
    private Integer rpNum;
--------------------------------------
    List<SeisStatsVO> contentList;
    SeisStatsVO total2D;
    SeisStatsVO total3D;
--------------------------------------
public SeisStatsSetVO getProjectStatInfo(ProjectDataQueryDTO dto) {
        List<StatSeisProject> projectStats = seisProjectDao.getProjectStats(new GridPara(), dto.getDim(), null, null, "", true);
        if (CollectionUtils.isEmpty(projectStats)) {
            return null;
        }

        return getSeisStatsSetVO(projectStats);
    }

    /**
     * 获取采集项目索引对应的统计信息
     * @param dto 查询对象参数，获取其中的idList属性
     * @return 采集项目对应的id列表对象
     */
    @Override
    public SeisStatsSetVO getProjectStatInfoByIds(ProjectDataQueryDTO dto) {
        String[] strings = new String[dto.getIdList().size()];
        for (int i = 0; i < dto.getIdList().size(); i++) {
            strings[i] = String.valueOf(dto.getIdList().get(i));
        }
        List<StatSeisProject> projectStats = seisProjectDao.getProjectStats(new GridPara(), strings);
        if (CollectionUtils.isEmpty(projectStats)) {
            return null;
        }

        return getSeisStatsSetVO(projectStats);
    }
--------------------------------------
 /**
     * 处理项目采集查询出来的数据 进行类型转换
     * @param projectStats 查询出来的项目集合列表
     * @return 返回给前端的对象
     */
    private SeisStatsSetVO getSeisStatsSetVO(List<StatSeisProject> projectStats) {
        StatSeisProject lv2d = createStatSeisProject(GlobalType.em2D);
        StatSeisProject lv3d = createStatSeisProject(GlobalType.em3D);
        int count2 = 0;
        int count3 = 0;
        for (StatSeisProject project : projectStats) {
            if (GlobalType.em2D.equals(project.getProjectType())) {
                sum(lv2d, project);
                count2 += 1;
            } else {
                sum(lv3d, project);
                count3 += 1;
            }
        }
        SeisStatsSetVO seisStatsSetVO = new SeisStatsSetVO();
        seisStatsSetVO.setContentList(SeisStatsMapper.INSTANCE.convert(projectStats));
        SeisStatsVO convert2D = SeisStatsMapper.INSTANCE.convert(lv2d);
        convert2D.setTotalProject(count2);
        seisStatsSetVO.setTotal2D(convert2D);

        SeisStatsVO convert3D = SeisStatsMapper.INSTANCE.convert(lv3d);
        convert3D.setTotalProject(count3);
        seisStatsSetVO.setTotal3D(convert3D);
        return seisStatsSetVO;
    }

    /**
     * 统计2D/3D信息
     * @param target 2D/3D信息存储对象
     * @param current 当前的对象
     */
    private void sum(StatSeisProject target, StatSeisProject current) {
        target.setSwathNum(target.getSwathNum() + current.getSwathNum());
        target.setSpsNum(target.getSpsNum() + current.getSpsNum());
        target.setLineLength(target.getLineLength() + current.getLineLength());
        target.setLineFullLength(target.getLineFullLength() + current.getLineFullLength());
        target.setOneFoldArea(target.getOneFoldArea() + current.getOneFoldArea());
        target.setShotArea(target.getShotArea() + current.getShotArea());
        target.setRpNum(target.getRpNum() + current.getRpNum());
        target.setSpNum(target.getSpNum() + current.getSpNum());
        target.setSpENum(target.getSpENum() + current.getSpENum());
        target.setSpVNum(target.getSpVNum() + current.getSpVNum());
        target.setSpONum(target.getSpONum() + current.getSpONum());
        target.setSpKlNum(target.getSpKlNum() + current.getSpKlNum());
        target.setLvlnum(target.getLvlnum() + current.getLvlnum());
        target.setLvlR(target.getLvlR() + current.getLvlR());
        target.setLvlU(target.getLvlU() + current.getLvlU());
        target.setLvlE(target.getLvlE() + current.getLvlE());
        target.setLvlO(target.getLvlO() + current.getLvlO());
        target.setLvlQ(target.getLvlQ() + current.getLvlO());
        target.setLvlLith(target.getLvlLith() + current.getLvlLith());
        target.setLvlDiv(target.getLvlDiv() + current.getLvlDiv());
        target.setLvlRecordNum(target.getLvlRecordNum() + current.getLvlRecordNum());
        target.setLvlImage(target.getLvlImage() + current.getLvlImage());
        target.setLvlExpResult(target.getLvlExpResult() + current.getLvlExpResult());
        target.setLvlbb(target.getLvlbb() + current.getLvlbb());
        target.setBigshotNum(target.getBigshotNum() + current.getBigshotNum());
        target.setFbtshotNum(target.getFbtshotNum() + current.getFbtshotNum());
        target.setFbtrecNum(target.getFbtrecNum() + current.getFbtrecNum());
        target.setSeisRecordNum(target.getSeisRecordNum() + current.getSeisRecordNum());
        target.setSeisRecordShotNum(target.getSeisRecordShotNum() + current.getSeisRecordShotNum());
        target.setTipRecordNum(target.getTipRecordNum() + current.getTipRecordNum());
        target.setSurveySectionNum(target.getSurveySectionNum() + current.getSurveySectionNum());
        target.setBadTrace(target.getBadTrace() + current.getBadTrace());
        target.setGeoNum(target.getGeoNum() + current.getGeoNum());
        target.setSeisBb(target.getSeisBb() + current.getSeisBb());
        target.setClcg(target.getClcg() + current.getClcg());
        target.setClsystem(target.getClsystem() + current.getClsystem());
        target.setClisn(target.getClisn() + current.getClisn());
        target.setClnet(target.getClnet() + current.getClnet());
        target.setClnetctrl(target.getClnetctrl() + current.getClnetctrl());
        target.setKtseis(target.getKtseis() + current.getKtseis());
        target.setDetPara(target.getDetPara() + current.getDetPara());
        target.setInsPara(target.getInsPara() + current.getInsPara());
        target.setShotPara(target.getShotPara() + current.getShotPara());
        target.setVibPara(target.getVibPara() + current.getVibPara());
        target.setAirPara(target.getAirPara() + current.getAirPara());
        target.setKtfieldS(target.getKtfieldS() + current.getKtfieldS());
        target.setKtfieldR(target.getKtfieldR() + current.getKtfieldR());
        target.setKtfieldG(target.getKtfieldG() + current.getKtfieldG());
        target.setCheckDocNum(target.getCheckDocNum() + current.getCheckDocNum());
        target.setGisMapFileNum(target.getGisMapFileNum() + current.getGisMapFileNum());
        target.setDocNum(target.getDocNum() + current.getDocNum());
    }
    private StatSeisProject createStatSeisProject(String ptype) {
        StatSeisProject statSeisProject = new StatSeisProject();
        statSeisProject.setProjectType(ptype);
        statSeisProject.setSwathNum(0);
        statSeisProject.setSpsNum(0);
        statSeisProject.setLineLength(0.0);
        statSeisProject.setLineFullLength(0.0);
        statSeisProject.setOneFoldArea(0.0F);
        statSeisProject.setShotArea(0.0);
        statSeisProject.setRpNum(0);
        statSeisProject.setSpNum(0);
        statSeisProject.setSpENum(0);
        statSeisProject.setSpVNum(0);
        statSeisProject.setSpONum(0);
        statSeisProject.setSpKlNum(0);
        statSeisProject.setLvlnum(0);
        statSeisProject.setLvlR(0);
        statSeisProject.setLvlU(0);
        statSeisProject.setLvlE(0);
        statSeisProject.setLvlO(0);
        statSeisProject.setLvlQ(0);
        statSeisProject.setLvlLith(0);
        statSeisProject.setLvlDiv(0);
        statSeisProject.setLvlRecordNum(0);
        statSeisProject.setLvlImage(0);
        statSeisProject.setLvlExpResult(0);
        statSeisProject.setLvlbb(0);
        statSeisProject.setBigshotNum(0);
        statSeisProject.setFbtshotNum(0L);
        statSeisProject.setFbtrecNum(0L);
        statSeisProject.setSeisRecordNum(0);
        statSeisProject.setSeisRecordShotNum(0);
        statSeisProject.setTipRecordNum(0);
        statSeisProject.setSurveySectionNum(0);
        statSeisProject.setBadTrace(0);
        statSeisProject.setGeoNum(0);
        statSeisProject.setSeisBb(0);
        statSeisProject.setClcg(0);
        statSeisProject.setClsystem(0);
        statSeisProject.setClisn(0);
        statSeisProject.setClnet(0);
        statSeisProject.setClnetctrl(0);
        statSeisProject.setKtseis(0);
        statSeisProject.setDetPara(0);
        statSeisProject.setInsPara(0);
        statSeisProject.setShotPara(0);
        statSeisProject.setVibPara(0);
        statSeisProject.setAirPara(0);
        statSeisProject.setKtfieldS(0);
        statSeisProject.setKtfieldR(0);
        statSeisProject.setKtfieldG(0);
        statSeisProject.setCheckDocNum(0);
        statSeisProject.setGisMapFileNum(0);
        statSeisProject.setDocNum(0);
        return statSeisProject;
    }
--------------------------------------
    /**
     * 获取项目统计信息
     * @param dto 项目数据查询参数DTO
     * @return 项目统计信息对象
     */
    @PostMapping("/projectStatInfo")
    public ResultData<SeisStatsSetVO> getProjectStats(@RequestBody ProjectDataQueryDTO dto) {
        return ResultData.success(seismicService.getProjectStatInfo(dto));
    }

    @PostMapping("/projectStatInfoByIds")
    public ResultData<SeisStatsSetVO> getStatOfIndexSeisProject(@RequestBody ProjectDataQueryDTO dto) {
        return ResultData.success(seismicService.getProjectStatInfoByIds(dto));
    }
--------------------------------------
